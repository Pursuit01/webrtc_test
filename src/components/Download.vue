<template>
  <div>
    <button @click="foo">点击下载</button>
  </div>
</template>

<script setup>
function downloadFile(url, fileName) {
  const chunkSize = 5 * 1024 * 1024; // 每个分片大小为1MB
  let start = 0;
  let end = chunkSize - 1;
  let index = 0;
  let chunks = [];

  function downloadChunk() {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.setRequestHeader("Range", `bytes=${start}-${end}`);
    xhr.responseType = "arraybuffer";

    xhr.onload = function () {
      if (xhr.status === 206 || xhr.status === 200) {
        chunks[index] = xhr.response;
        start += chunkSize;
        end += chunkSize;
        index++;

        if (xhr.status === 200) {
          // 所有分片下载完成
          mergeChunks(fileName);
        } else {
          // 下载下一个分片
          downloadChunk();
        }
      }
    };

    xhr.send();
  }

  function mergeChunks(fileName) {
    const blob = new Blob(chunks);
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = fileName;
    a.click();
  }

  downloadChunk();
}
const foo = () => {
  // 使用示例
  const url =
    "https://192.168.12.243:9000/media-publish/1/wzy1_1689755046930_1.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=dev%2F20230821%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230821T013151Z&X-Amz-Expires=7200&X-Amz-SignedHeaders=host&X-Amz-Signature=9647558e4fb45df50e991a56e467a9635e67aef89270f78a5af74a7655bbf569"; // 资源文件URL
  const fileName = "file.mp4"; // 下载后的文件名
  downloadFile(url, fileName);
};
</script>

<style lang="scss" scoped></style>
