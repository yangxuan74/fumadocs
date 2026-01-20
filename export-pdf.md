如何导出PDF，参考https://www.fumadocs.dev/docs/guides/export-pdf

1. 在根目录下，创建scripts文件夹，里面新建一个export-pdf.ts文件。


import puppeteer from 'puppeteer';
import fs from 'node:fs/promises';
import path from 'node:path';

const browser = await puppeteer.launch({
executablePath: 'C:\\Users\\admin\\Desktop\\chrome-win\\chrome.exe' // 替换为您的 Chrome 路径，puppeteer安装参考https://www.cnblogs.com/zhangyaolan/p/12590028.html，即npm i  puppeteer --ignore-scripts ，然后下载chrome浏览器，这块配置chrome.exe路径。
});
const outDir = 'pdfs';
// update this
const urls = ['/docs/ui', '/docs/ui/customisations'];

async function exportPdf(pathname: string) {
  const page = await browser.newPage();
  await page.goto('http://localhost:3000' + pathname, {
    waitUntil: 'networkidle2',
  });

  await page.pdf({
    path: path.join(outDir, pathname.slice(1).replaceAll('/', '-') + '.pdf'),
    width: 950,
    printBackground: true,
  });

  console.log(`PDF generated successfully for ${pathname}`);
  await page.close();
}

await fs.mkdir(outDir, { recursive: true });
await Promise.all(urls.map(exportPdf));
await browser.close();


安装bun。npm install bun
