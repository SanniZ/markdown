### 加载vmm, trustyOS和kernel流程



```flow
st=>start: 上电初始化
loadVmm=>operation: 加载vmm
loadTrustyOS=>operation: 加载trustyOS
loadKernel=>operation: 加载kernel
checkVmm=>condition: 加载vmm成功?
checkTrustyOS=>condition: 加载trustyOS成功?
checkKernel=>condition: 加载kenrel成功?
runTrustyOS=>operation: 运行trustyOS
runKernel=>operation: 运行kernel
exit=>operation: 退出
st->loadVmm->checkVmm
checkVmm(no)->exit
checkVmm(yes)->loadTrustyOS->checkTrustyOS
checkTrustyOS(no)->exit
checkTrustyOS(yes)->loadKernel->checkKernel
checkKernel(no)->exit
checkKernel(yes)->runKernel

```