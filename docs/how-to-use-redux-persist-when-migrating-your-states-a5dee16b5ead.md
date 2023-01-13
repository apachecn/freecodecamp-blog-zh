# 迁移状态时如何使用 Redux Persist

> 原文：<https://www.freecodecamp.org/news/how-to-use-redux-persist-when-migrating-your-states-a5dee16b5ead/>

存储一直是构建应用不可或缺的一部分。在为我们公司构建 webapp 时，我需要一种可靠、易用、可根据需求配置的方法来将我的状态保存在存储中。

谢天谢地，这个图书馆解决了我所有的问题！

这篇文章是基于我在一个项目中遇到的一个问题。让我们深潜一下，了解一下图书馆是怎么帮我解决的。

如果你还没有使用过 [redux-persist](https://github.com/rt2zz/redux-persist) ，那么一定要阅读文档，因为它们是不言自明的。如果你想知道为什么你应该使用这个库，去看看这篇[文章](https://medium.com/async-la/redux-persist-your-state-7ad346c4dd07)——[作者](https://twitter.com/rt2zz)自己的精彩介绍！

### 问题

让我们举一个例子，我想在本地存储中保存一个缩减器:

```
//Reducer
reducerA: {  
    engine: {    
        model: "F5AAA",    
        manufacturer: "Ferrari"  
    },  
    tyre: {    
        model: "T123",   
		manufacturer: "MRF",    
		owner: {      
            details: {        
                name: "Zack",        
				age: "26"            
            }    
        }  
    },  
	condition: "prime"
}
```

```
//View
class TestComponent extends React.Component {  
    render() {    
        const model = someStateOfReducerA.tyre.model    
        const manufacturer = someStateOfReducerA.tyre.manufacturer

		return (      
            <div>{model}</div>      
            <div>{manufacturer}</div>    
        )  
    }
}

//Reducer in localStorage
reducerA: {  
    engine: {    
        model: "F5AAA",    
		manufacturer: "Ferrari"  
    },  
	tyre: {    
        model: "T123",    
		manufacturer: "MRF",    
		owner: {      
            details: {        
                name: "Zack",        
				age: "26"            
            }    
        }  
    },  
	condition: "prime"
}
```

现在这个缩减器已经存在于我们客户的设备中。那么，如果我给我们的 **reducerA** 引入一个新的键，会发生什么呢？

```
reducerA: {  
	engine: {    
    	model: "F5AAA",    
	    manufacturer: "Ferrari"  
   	},  
    tyre: {    
    	model: "T123",    
        manufacturer: "MRF",    
        owner: {      
        	details: {        
            	name: "Zack",        
                age: "26",        
                address: "CA" // NEW KEY ADDED
			}    
		}  
	},  
    condition: "prime"
}
```

假设我们有一个视图来呈现我们新引入的键的值:

```
//View with new key address
class TestComponent extends React.Component {  
    render() {    
        const model = someStateOfReducerA.tyre.model    
        const manufacturer = someStateOfReducerA.tyre.manufacturer    
        const address = someStateOfReducerA.tyre.owner.details.address

		return (      
            <div>{model}</div>      
            <div>{manufacturer}</div>      
            <div>{address}</div>
		)  
    }
}
```

当我用新引入的键加载应用程序时，视图呈现失败。它会抛出一个错误，指出:

```
Cannot render address of undefined
```

发生这种情况是因为客户端的存储与我们的应用程序重新加载期间初始化的 rootReducer 同步。

即使我们引入了新的密钥，客户的存储也从未收到它。它用存储中的旧值初始化我们的 rootReducer，这个地址从来不存在，并导致我们的组件呈现失败。

### 解决办法

这就引出了一个众所周知的概念:数据库的迁移。

> 每当需要将**数据库的**模式更新或恢复到某个较新或较旧的版本时，就会在**数据库**上执行模式**迁移**。**迁移**通过使用模式**迁移**工具— [维基百科](https://en.wikipedia.org/wiki/Schema_migration)以编程方式执行

LocalStorage 实际上是一个小型的键值对数据库。Redux Persist 做得很漂亮。如果您查看用这个库初始化的项目，它已经使用了一个**默认版本-1** 。看看下面从 Chrome 开发工具的应用程序标签中截取的截图。

![0rJmD7xq6mgOnUokqih4WTMy2F6Kd3GmgalV](img/4de539886999705412293a23197d14d7.png)

localStorage in Chrome Dev Tool

这真的很好！该库已经为我们维护了一个默认版本，这样我们就可以在将来集成迁移特性。

关键是在 rootReducer 中配置持久化配置。

```
export const persistConfig = {  
    key: 'testApp',  
    version: 0, //New version 0, default or previous version -1  
    storage,  
    debug: true,  
    stateReconciler: autoMergeLevel2,  
    migrate: createMigrate(migrations, { debug: true })
}
```

我们必须将版本更新到 0，以便将我们的存储从-1 迁移到 0。

接下来，我们编写迁移，让我们的存储知道有更新。

```
const migrations = {  
    0: (state) => {    
        return {      ...
			state,      
			tyre: {        ...
				state.tyre,        
				owner: {          ...
					state.tyre.owner,          
					details: {
                        state.tyre.owner.details,
                        address: "CA" //New Key added for migration
                    }
				}      
			}    
		}  
    }
}
```

然后在上面提到的持久化配置中使用**迁移**:

```
migrate: createMigrate(migrations, { debug: true })
```

因此，当我们重新加载我们的应用程序时，我们的应用程序会经历一个协调阶段，在这个阶段，存储与新更新的 reducer 保持同步。

### 结论

当您发布新版本时，上面的配置将始终保持客户端应用程序的更新。非常重要的是，我们在制作离线第一应用程序时要小心这一点。

一旦你理解了基本的概念和技术，事情就简单了。我希望这篇文章能帮助您理解在存储中管理状态版本的重要性:)

*在 **[twitter](https://twitter.com/daslusan)** 上关注我，获取更多关于新文章的更新，并了解最新的前端开发。也在 twitter 上分享这篇文章，帮助其他人了解它。分享就是关爱 **^_^.***

### 一些有用的资源

1.  [https://github . com/rt2zz/redux-persist/blob/master/docs/API . MD](https://github.com/rt2zz/redux-persist/blob/master/docs/api.md)
2.  [https://medium . com/@ clrksanford/persist-ence-is-key-using-redux-persist-to-store-your-state-in-local storage-AC 6a 000 aee 63](https://medium.com/@clrksanford/persist-ence-is-key-using-redux-persist-to-store-your-state-in-localstorage-ac6a000aee63)
3.  [https://medium . com/async-la/redux-persist-your-state-7ad 346 C4 DD 07](https://medium.com/async-la/redux-persist-your-state-7ad346c4dd07)