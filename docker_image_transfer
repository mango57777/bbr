#!/bin/bash

# 显示菜单
echo "请选择操作："
echo "1. 导入 Docker 镜像"
echo "2. 导出 Docker 镜像"
echo -n "输入对应数字："
read choice

if [ "$choice" == "1" ]; then
    # 导入 Docker 镜像的代码段
    EXPORT_DIR="/root"
    EXPORT_FILE_SUFFIX="_export.tar"

    echo "可用的导入文件列表："
    export_files=($(ls -1 ${EXPORT_DIR}/*${EXPORT_FILE_SUFFIX} 2>/dev/null))
    for i in "${!export_files[@]}"; do
        echo "$((i+1)): ${export_files[i]}"
    done

    echo -n "请选择要导入的文件（输入对应序号）："
    read file_number

    selected_file="${export_files[file_number-1]}"

    if [ -z "$selected_file" ]; then
        echo "无效的选择。退出脚本。"
        exit 1
    fi

    docker load -i "$selected_file"
    echo "成功导入镜像文件： $selected_file"

elif [ "$choice" == "2" ]; then
    # 导出 Docker 镜像的代码段
    echo "可用的 Docker 镜像列表："
    docker images --format ": {{.Repository}}:{{.Tag}}" | cat -n

    echo -n "请选择要导出的镜像（输入对应序号）："
    read image_number

    if ! [[ "$image_number" =~ ^[0-9]+$ ]] ; then
       echo "无效的输入：请输入一个数字。退出脚本。"
       exit 1
    fi

    selected_image=$(docker images --format "{{.Repository}}:{{.Tag}}" | awk -v n=$image_number 'NR==n {print $0}')

    if [ -z "$selected_image" ]; then
        echo "无效的选择。退出脚本。"
        exit 1
    fi

    # 替换斜杠和冒号为下划线
    export_file="/root/$(echo "$selected_image" | tr '/:' '_')_export.tar"
    echo "选定的镜像名称： $selected_image"
    echo "导出文件路径： $export_file"

    docker save -o "$export_file" "$selected_image"
    echo "成功导出镜像到 $export_file"

else
    echo "无效的选择。退出脚本。"
    exit 1
fi
