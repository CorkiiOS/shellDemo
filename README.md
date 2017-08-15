# shellDemo

# #!/bin/sh
# echo "~~~~~~~~~~~~开始执行脚本~~~~~~~~~~~~~~"


# #开始时间

# beginTime=`date +%s`
# DATE=`date '+%Y-%m-%d-%T'`

# #工程绝对路径
# project_path=$(pwd)

# #需要编译的 targetName
# project_name="周翼单车"

# scheme_name = "周翼单车"

# #编译模式 工程默认有 Debug Release 
# development_mode=Release

# #编译路径
# build_path=${project_path}/build

# #输出的ipa目录
# ipaPath=${project_path}/ipa/${development_mode}

# #PROVISIONING_PROFILE = "2dff8e28-342a-416e-994b-bcf4dda3653a";
# #PROVISIONING_PROFILE_SPECIFIER = zhouyi_124;
# #证书名
# codeSignIdentity="iPhone Distribution: ZhouYI Hebei Network Technology Co., Ltd. (NABY5B5X89)"
# #描述文件
# provisioningProfile="2dff8e28-342a-416e-994b-bcf4dda3653a"

# ##苹果账号
# AppleID="xxxx"
# AppleIDPWD="xxxx"

# #导出ipa 所需plist
# ADHOCExportOptionsPlist=./ADHOCExportOptionsPlist.plist
# AppStoreExportOptionsPlist=./AppStoreExportOptionsPlist.plist

# ExportOptionsPlist=${ADHOCExportOptionsPlist}

# echo "~~~~~~~~~~~~~选择打包方式~~~~~~~~~~~~~~~"
# echo "1 sd-hoc(默认) "
# echo "2 AppStore "

# # 读取用户输入并存到变量里
# read parameter
# sleep 0.5
# method="$parameter"

# # 判读用户是否有输入

# if [[ -n "method" ]]; then
# 	#statements
# 	echo "~~~~~~~~~~~~检测输入~~~~~~~~~~~~~~~"

# 	if [[ "$method" = "1" ]]; then
# 		#statements
# 		provisioningProfile=${provisioningProfile}
# 		ExportOptionsPlist=${ADHOCExportOptionsPlist}

# 		echo "~~~~~~~~~~~~~~~测试-adhoc~~~~~~~~~~~"
# 	elif [[ "$method" = "2" ]]; then
# 			#statements
# 			provisioningProfile="app.pp"
# 			ExportOptionsPlist=${AppStoreExportOptionsPlist}
# 		echo "~~~~~~~~~~~~~~~AppStore~~~~~~~~~~~"

# 	else
# 		echo "参数无效"
# 		exit 1
# 	fi
# else 
# 	ExportOptionsPlist=${ADHOCExportOptionsPlist}
# fi

# echo "~~~~~~~~~~~~~~~~~1.clean~~~~~~~~~~~~~~~~~~~~"

# # 清理 避免出现一些莫名的错误
# xcodebuild \
# clean -configuration ${development_mode} -quiet || exit
# echo "~~~~~~~~~~~~~~~~~clean finshed~~~~~~~~~~~~~~"

# #编译
# echo '*** 正在 编译工程 For '${development_mode}

# xcodebuild archive -workspace ${project_path}/${project_name}.xcworkspace \
# -scheme ${scheme_name} \
# -configuration ${development_mode} \
# -archivePath ${build_path}/${project_name}.xcarchive \
# CODE_SIGN_IDENTITY="${codeSignIdentity}" \
# PROVISIONING_PROFILE="${provisioningProfile}"

# echo '*** 编译完成 ***'

# echo "~~~~~~~~~~~~~~~~~~检测是否构建成功~~~~~~~~~~~~~~~"
# # xcarchive 实际是一个文件夹不是一个文件所以使用 -d 判断

# if [[ -d "$archivePath" ]]; then
# 	#statements
# 	echo "构建成功"
# 	else
# 		echo "构建失败"
# 		rm -rf %buildPath
# 		exit 1

# fi
# endTime=`date +%s`
# ArchiveTime="构建时间$[ endTime - beginTime ]秒"

# echo "~~~~~~~~~~~~~~~~~倒出ipa~~~~~~~~~~~~~~~~~~"

# beginTime=`date +%s`

# xcodebuild \
# -exportArchive \
# -archivePath ${build_path}/${targetName}.xcarchive \
# -configuration ${development_mode} \
# -exportOptionsPlist ${ExportOptionsPlist} \
# -exportPath ${ipaPath}

# echo "~~~~~~~~~~~~~~检测是否成功倒出ipa~~~~~~~~~~~~~"

# if [[ -f "$ipaPath" ]]; then
# 	#statements
# 	echo "~~~~~~~~~~~~~~~~~~倒出成功~~~~~~~~~~~~~~~~~~~~~"
# 	else
# 		echo "~~~~~~~~~~~~~~~倒出失败~~~~~~~~~~~~~~~~~~~"
# 		endTime=`date +%s`
# 		echo "ArchiveTime"
# 		echo "倒出ipa时间$[ endTime - beginTime ]秒"
# 		exit 1
# fi

# endTime=`date +%s`


#! /bin/bash
# created by Ficow Shen

#工程绝对路径
project_path=$(pwd)

#工程名
project_name="周翼单车"

#打包模式 Debug/Release
development_mode=Release

#scheme名
scheme_name="周翼单车"

#PROVISIONING_PROFILE = "2dff8e28-342a-416e-994b-bcf4dda3653a";
#PROVISIONING_PROFILE_SPECIFIER = zhouyi_124;
#证书名
codeSignIdentity="iPhone Distribution: ZhouYI Hebei Network Technology Co., Ltd. (NABY5B5X89)"
#描述文件
provisioningProfile="2dff8e28-342a-416e-994b-bcf4dda3653a"

#build文件夹路径
build_path=${project_path}/build

#plist文件所在路径
exportOptionsPlistPath=${project_path}/exportOptions.plist

#导出.ipa文件所在路径
exportFilePath=${project_path}/ipa/${development_mode}

echo "~~~~~~~~~~~~~选择打包方式~~~~~~~~~~~~~~~"
echo "1 sd-hoc(默认) "
echo "2 AppStore "

# 读取用户输入并存到变量里
read parameter
sleep 0.5
method="$parameter"

# 判读用户是否有输入
if [[ -n "method" ]]; then
	#statements
	if [[ "$method" = "1" ]]; then
		#statements
		echo "~~~~~~~~~~~~~adhoc~~~~~~~~~~~~~"

	elif [[ "$method" = "2" ]]; then
			#statements
			echo "~~~~~~~~~~~~~~~AppStore~~~~~~~~~~"
	else
		echo "~~~~~~~不合理～～～～～～～～"

	fi
	
	else
		echo "~~~~~~~~~没有输入～～～～～～～～～～"
fi

echo "～～～～～～～cleaning。。。"
xcodebuild \
clean -configuration ${development_mode} -quiet  || exit 
echo "～～～～～～～～clean finished ～～～"

echo '*** 正在 编译工程 For '${development_mode}
xcodebuild \
archive -workspace ${project_path}/${project_name}.xcworkspace \
-scheme ${scheme_name} \
-configuration ${development_mode} \
CODE_SIGN_IDENTITY="${codeSignIdentity}" \
PROVISIONING_PROFILE="${provisioningProfile}" \
-archivePath ${build_path}/${project_name}.xcarchive -quiet  || exit
echo '*** 编译完成 ***'



echo '*** 正在 打包 ***'
xcodebuild -exportArchive -archivePath ${build_path}/${project_name}.xcarchive \
-configuration ${development_mode} \
-exportPath ${exportFilePath} \
-exportOptionsPlist ${exportOptionsPlistPath} \
-quiet || exit

if [ -e $exportFilePath/$scheme_name.ipa ]; then
    echo "*** .ipa文件已导出 ***"
    open $exportFilePath
else
    echo "*** 创建.ipa文件失败 ***"
fi
echo '*** 打包完成 ***'
