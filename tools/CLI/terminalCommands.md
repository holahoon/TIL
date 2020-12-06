# Linux Terminal Commands

#### Current directory
```sh
pwd
```

#### Create new directory
```sh
mkdir {{name_of_your_directory}}
```

#### Change directory
```sh
cd {{relative_path_of_new_directory}}
```

#### List all files in a directory
```sh
ls
```

#### List all files in a directory including hidden files
```sh
ls -a
```

#### List all files with timestamps, sizes, and permissions
```sh
ls -l
```

#### List all hidden files with timestamps, sizes, and permissions
```sh
ls -la
```

#### Create a new file
```sh
touch {{name_of_file}}
```

#### Move a file
```sh
mv {{name_of_file}} {{relative_path_of_new_location}}
```

#### Rename a file
```sh
mv {{name_of_file}} {{name_of_new_file}}
```

#### Copy a file
```sh
cp {{name_of_file}} {{relative_path_of_new_location}}
```

#### Copy a directory
```sh
cp -r {{name_of_file}} {{relative_path_of_new_location}}
```

#### Delete a file
```sh
rm {{name_of_file}}
```

#### Delete a directory
```sh
rm -r {{name_of_file}}
```

#### Read the whole file content
```sh
more {{name_of_file}}
```

#### Read the last ten lines usually for log files
```sh
tail {{name_of_file}}
```

#### Search inside a file
```sh
grep {{text_to_search}} {{name_of_file}}
```

#### Save the results of the command in a file and overwrite it
```sh
grep {{text_to_search}} {{name_of_file}} > {{filename}}
```

#### Append the results of the command in a file
```sh
grep {{text_to_search}} {{name_of_file}} >> {{filename}}
```