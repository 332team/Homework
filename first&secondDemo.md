##由于任务二包含任务一，所以只上传了任务二；这两个程序都有老师给出的源码作为基础，所以并不困难。
我只在一开始写程序的时候遇到了一个难处，我把整个图片的输出写到了区域颜色改变的代码的后面，
导致所输出的图片一直没有改变。再后来询问同学并改变了编码顺序之后终于做出来。##
```
#include "stdafx.h"
#include "iostream"
using namespace std;
#include "./gdal/gdal_priv.h"
#pragma comment(lib,"gdal_i.lib")

int main()
{
	GDALDataset* poSrcDS;
	GDALDataset* poDstDS;
	int imgXlen, imgYlen,StartX, StartY,tmpXlen, tmpYlen;
	char* srcPath = "square.jpg";
	char* dstPath = "rest.tif";
	GByte* buffTmp1;
	GByte* buffTmp;
	int i,j,s, bandNum;
	GDALAllRegister();//打开图像；
	poSrcDS = (GDALDataset*)GDALOpenShared(srcPath, GA_ReadOnly);
	//获取图像数据
	StartX = 100;
	StartY = 100;
	tmpXlen=200;
	tmpYlen = 150;
	imgXlen = poSrcDS->GetRasterXSize();
	imgYlen = poSrcDS->GetRasterYSize();
	bandNum = poSrcDS->GetRasterCount();
	//
	cout << "Image X Length:" << imgXlen << endl;
	cout << "Image Y Length:" << imgYlen << endl;
	cout << "Band number:" << bandNum << endl;
	//
	buffTmp = (GByte*)CPLMalloc(imgXlen*imgYlen * sizeof(GByte));
	buffTmp1 = (GByte*)CPLMalloc(tmpXlen*tmpYlen * sizeof(GByte));
	//分配内存
	poDstDS = GetGDALDriverManager()->GetDriverByName("GTiff")->Create(dstPath, imgXlen, imgYlen, bandNum, GDT_Byte, NULL);
	//创建输出图像
	for (s = 0; s< bandNum; s++) {
		poSrcDS->GetRasterBand(s + 1)->RasterIO(GF_Read, 0, 0, imgXlen, imgYlen, buffTmp, imgXlen, imgYlen, GDT_Byte, 0, 0);
		poDstDS->GetRasterBand(s + 1)->RasterIO(GF_Write, 0, 0, imgXlen, imgYlen, buffTmp, imgXlen, imgYlen, GDT_Byte, 0, 0);
		printf("……band %d processing……\n", s);
	}
	poSrcDS->GetRasterBand( 1)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++) 
		for (j= 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)255;
		}
	poDstDS->GetRasterBand( 1)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);

	StartX = 300;
	StartY = 300;
	tmpXlen = 100;
	tmpYlen = 50;
	poSrcDS->GetRasterBand(1)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)255;
		}
	poDstDS->GetRasterBand(1)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	poSrcDS->GetRasterBand(2)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)255;
		}
	poDstDS->GetRasterBand(2)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	poSrcDS->GetRasterBand(3)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)255;
		}
	poDstDS->GetRasterBand(3)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	StartX = 500;
	StartY = 500;
	tmpXlen = 50;
	tmpYlen = 100;
	poSrcDS->GetRasterBand(1)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)0;
		}
	poDstDS->GetRasterBand(1)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	poSrcDS->GetRasterBand(2)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)0;
		}
	poDstDS->GetRasterBand(2)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	poSrcDS->GetRasterBand(3)->RasterIO(GF_Read, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	for (i = 0; i < tmpYlen; i++)
		for (j = 0; j< tmpXlen; j++)
		{
			buffTmp1[i*tmpXlen + j] = (GByte)0;
		}
	poDstDS->GetRasterBand(3)->RasterIO(GF_Write, StartX, StartY, tmpXlen, tmpYlen, buffTmp1, tmpXlen, tmpYlen, GDT_Byte, 0, 0);
	CPLFree(buffTmp);
	CPLFree(buffTmp1);
	GDALClose(poDstDS);
	GDALClose(poSrcDS);
	system("PAUSE");
	return 0;
}

```
