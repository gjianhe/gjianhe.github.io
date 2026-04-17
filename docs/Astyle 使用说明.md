# Astyle 使用说明

## 单个文件格式化

> `astyle --style=风格 代码文件`

## 给定文件夹内的所有代码文件格式化

> `astyle --style=风格 src/*.c include/*.h`

## 当前路径下所有代码文件都格式化

> `astyle --style=风格 -r "*.c" "*.h"`

## 排除指定文件夹

> `astyle --style=风格 -r "*.c" "*.h" --exclude=3rdparty`

## 当前路径下所有代码根据配置文件格式化
> `astyle --options=as.cfg -r "*.c" "*.h"`

## 禁止块格式化

> `// *INDENT-OFF*`
> `// *INDENT-ON*`

## 禁止行格式化

> `// *NOPAD*`

