# Motorola Edge 50 Neo (Vienna) kernel manifest

## create folder & sync repo
<pre>bash cd ~/
mkdir -p Vienna && cd Vienna
repo init -u https://github.com/nooobas/vienna_kernel_manifest.git -m default.xml
repo sync -j$(nproc)</pre>

## create symlinks
<pre>bash ln -s kernel_device_modules-6.1 kernel_device_modules-mainline
ln -s kernel_device_modules-6.1 kernel_device_modules</pre>

## copy vienna config
<pre>bash mkdir -p kernel_device_modules-6.1/kernel/configs/ext_config
cp -r kernel_device_modules-6.1/arch/arm64/configs/ext_config/moto-mgk_64_k61-vienna.config \
   kernel_device_modules-6.1/kernel/configs/ext_config/moto-mgk_64_k61-vienna.config</pre>

## build kernel
<pre>bash bazel build //kernel-6.1:kernel --//:kernel_version=6.1 --//:internal_config=true</pre>

## build device modules
<pre>bash export DEFCONFIG_OVERLAYS="ext_config/moto-mgk_64_k61-vienna.config"
bazel build //kernel_device_modules-6.1:mgk_64_k61.user</pre>

## clean sources
<pre>bash bazel clean --expunge </pre>
