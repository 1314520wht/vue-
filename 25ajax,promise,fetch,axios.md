### 数据请求。ajax,promise,fetch,axios

#### 1.jquery的ajax

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/jquery@3.5.1/dist/jquery.js"></script>
</head>
<body>
    
    <script>
        $.ajax({
            type:"POST",   //get提交方式
            url:url,   //提交地址
            data:data,  //发送的数据
            dataType:dataType,  //接受的数据类型
            success:function(date){},  //成功时的回调函数
            error:function(data){},  //失败时的回调函数  
        })
 
        //1.get请求        
        $.ajax({
            type:"GET",
            url:"http://jsonplaceholder.typicode.com/users" ,
            data:"",
            dataType:"",
            success:function(data){
                console.log("success")
            },
            error:function(data){
                console.log("error")
            }
        })
        

        //2.post请求
        $.ajax({
            type:"POST",
            url:"http://jsonplaceholder.typicode.com/users",
            data:{name:"xin",age:"18"},
            dataType:"json",
            success:function(data){
                console.log(data)
            },
            error:function(data){
                console.log("error")
            }
        })
        //fetch是基于ajax分装的。
    </script>
</body>
</html>
```

#### 2.promise

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        //promise  把异步操作转为同步流程表现出来。3个状态。

        //例子1
        //成功状态
        let promise1 = new Promise((res,rej)=>{
             //当里面不传状态的时候，默认pending 等待中
            //res()   //成功状态    //成功了  成功了  成功了
             rej()  //失败状态    //出错了
        })

        promise1
            .then(()=>{console.log("成功了")})
            .then(()=>{console.log("成功了")})
            .then(()=>{console.log("成功了")})
            .catch(()=>{console.log("例子1出错了")})
        // 例子1出错了   



        //例子2  定时器执行
        let promise2 = new Promise((res,rej)=>{
            setTimeout(()=>{
                res()
            },3000)
        })
        promise2
            .then(()=>{console.log("例子2成功了")})
            .then(()=>{console.log("例子2成功了")})
            .then(()=>{console.log("例子2成功了")})
            .catch(()=>{console.log("例子2出错")})

        //例子2成功了   例子2成功了   例子2成功了



        //例子3 状态里面传内容
        let p1 = new Promise((res,rej)=>{
                res()
        })
        p1
            .then(()=>{console.log("p1成功了")})
            .catch(()=>{console.log("p1出错了")}) 

        let p2 = new Promise((res,rej)=>{
                res()
        })
          
        p2
            .then(()=>{console.log("p2成功了")})
            .catch(()=>{console.log("p2出错了")})   
        //p1成功了     p2成功了



        //例子4 状态里面传内容.根据传入的内容，查看状态，进而影响总体状态。传入内容的最后执行。
        let p3 = new Promise((res,rej)=>{
                res(p4)  //浏览器执行的时候未找p4，所以为rej
        })
        p3
            .then(()=>{console.log("p3成功了")})
            .catch(()=>{console.log("p3出错了")}) 

        let p4 = new Promise((res,rej)=>{
                res()
        })
          
        p4
            .then(()=>{console.log("p4成功了")})
            .catch(()=>{console.log("p4出错了")})   

        //p4成功了    p3出错了   


        //例子5
        let p5 = new Promise((res,rej)=>{
                res()
        })     
        p5
            .then(()=>{console.log("p5成功了")})
            .catch(()=>{console.log("p5出错了")})  

        let p6 = new Promise((res,rej)=>{
                res(p5)
        })
        p6
            .then(()=>{console.log("p6成功了")})
            .catch(()=>{console.log("p6出错了")}) 
        // p5成功了  p6成功了    



        //例子6
        let p7 = new Promise((res,rej)=>{
                rej()
        })     
        p7
            .then(()=>{console.log("p7成功了")})
            .catch(()=>{console.log("p7出错了")})  

        let p8 = new Promise((res,rej)=>{
                res(p7)
        })
        p8
            .then(()=>{console.log("p8成功了")})
            .catch(()=>{console.log("p8出错了")}) 
        // p7出错了        p8出错了


        //例子7
        let p9 = new Promise((res,rej)=>{
                rej(p10)
        })     
        p9
            .then(()=>{console.log("p9成功了")})
            .catch(()=>{console.log("p9出错了")})  

        let p10 = new Promise((res,rej)=>{
                res()
        })
        p10
            .then(()=>{console.log("p10成功了")})
            .catch(()=>{console.log("p10出错了")}) 
            .catch(()=>{console.log("p10出错了")}) 
        //p10成功了    p9出错了 



        //例子8
        let p11 = new Promise((res,rej)=>{
                rej(p12)
        })     
        p11
            .then(()=>{console.log("p11成功了")})
            .catch(()=>{console.log("p11出错了")})  

        let p12 = new Promise((res,rej)=>{
                res()
        })
        p12
            .then(()=>{console.log("p12成功了")})
            .catch(()=>{console.log("p12出错了")}) 
        //p12成功了     p11出错了
    </script>
</body>
</html>
```

#### 3.fetch

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        //使用方式  fetch(url)
        //               .then(res=>res.json())
        //               .then(data=>console.log(data))
        //               .catch(err=>console.log(err))

        let url = "http://jsonplaceholder.typicode.com/posts"
        
        fetch(url)
           .then(res=>res.json())  //res是响应的信息，转为json格式
           .then(data =>console.log(data))  //data是响应的数据
           .catch(err=>console.log(err))
    </script>
</body>
</html>
```

#### 4.promise与fetch结合封装

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        //使用promise 与fetch进行封装。promise可拿到状态，fetch可拿到数据
        class HTTP{
            //get(url)
            get(url){
                return  new Promise((res,rej)=>{
                    fetch(url)
                        .then(res => res.json())
                        .then(data =>res(data))
                        .catch(err =>rej(err))
                })
            }
 

            //post(url,datasd)
            post(url,data){
                return  new Promise((res,rej)=>{
                    fetch(url,{
                        method:"POST",
                        headers:{
                            "Content-type":"application/json"
                        },
                        body:JSON.stringify(data)
                    })
                    .then(res => res.json())
                    .then(data =>res(data))
                    .catch(err =>rej(err))
                })
            }

            
            put(url,data){
                return  new Promise((res,rej)=>{
                    fetch(url,{
                        method:"PUT",
                        headers:{
                            "Content-type":"application/json"
                        },
                        body:JSON.stringify(data)
                    })
                    .then(res => res.json())
                    .then(data =>res(data))
                    .catch(err =>rej(err))
                })
            }


            delete(url,data){
                return  new Promise((res,rej)=>{
                    fetch(url,{
                        method:"PUT",
                        headers:{
                            "Content-type":"application/json"
                        },
                        body:JSON.stringify(data)
                    })
                    .then(res => res.json())
                    .then(data =>res(data))
                    .catch(err =>rej(err))
                })
            }
        }
    </script>
    <script>
        const http = new HTTP;
        var url = "http://jsonplaceholder.typicode.com/users"
        //1.get请求数据
        http.get(url)
            // .then(res =>res.json())
            .then(data =>{
                console.log(data)   //拿到了数据
            })
            .catch(err =>{
                console.log(err)
            })



        //2.post提交数据
        let data = {
            name:"信心",
            age:18
        }
        http.post(url,data)
            .then(data => console.log(data))
            .catch(err => console.log(err))


        //3.修改数据
        let data1 = {
            name:"haha",
            age:11
        }
        http.put(url + "/3",data1)  //修改id为3的数据
            .then(data1 => console.log(data1))
            .catch(err => console.log(err))

        
        //4.删除数据
       
        http.delete(url + "/3",data1)  //修改id为3的数据
            .then(data1 => console.log(data1))
            .catch(err => console.log(err))
    </script>
</body>
</html>
```

#### 5.axios

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>
<body>
    <script>
        axios.get("http://jsonplaceholder.typicode.com/posts")
            .then(res =>{
                console.log(res)
                console.log(res.data)
            })
            .catch(err =>{
                console.log(err)
            })

        axios.post("http://jsonplaceholder.typicode.com/posts?userId=1",{  //会找到userId=1的数据，用data替换。
            name:122222,
            username:2333333,
            age:999
            })   
            .then(res =>{
                 console.log(res)
             }) 
            .catch(err =>{
                console.log(err)
            }) 
           


         //post与put差别不是很大，有时会混用。   
    </script>
</body>
</html>
```

