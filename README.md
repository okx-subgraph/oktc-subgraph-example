## 启动方式

* 依赖安装
    * yarn install
* 编译 & 部署
    * 基于 subgraph 的 cli 命令进行，参见 subgraph[官方文档](https://thegraph.com/docs/en/developer/quick-start/)
    * 具体步骤：
        * 编译ts代码到types目录
            * ```text
        yarn codegen
        ```
        * 编译整个工程到build目录
            *  ```text
         yarn build
            ```
        * 往graph节点创建子图
            * ```text
         graph create oeswap/info-v1-subgraph-test --node http://10.254.22.165:8020 
        ```
        * 将编译后wasm代码发布到ipfs并发布子图到graph节点
            * ```text
         graph deploy oeswap/info-v1-subgraph-test --ipfs http://10.254.22.165:5001 --node http://10.254.22.165:8020
        ```

