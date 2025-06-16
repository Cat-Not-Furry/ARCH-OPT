#!/bin/bash

RED='\[\e[0;31m\]'
temp=$(sensors | grep "Core 0" | awk '{gsub("[+°C]", "", $3); print $3}')
temp_num=$(echo "$temp" | sed 's/[^0-9.]//g')

if (($(echo "$temp_num <= 25.0" | bc -l))); then
  echo " $temp°C "
elif (($(echo "$temp_num <= 40.0" | bc -l))); then
  echo " $temp°C "
elif (($(echo "$temp_num <= 50.0" | bc -l))); then
  echo " $temp°C "
elif (($(echo "$temp_num <= 60.0" | bc -l))); then
  echo " $temp°C "
elif (($(echo "$temp_num <= 69.0" | bc -l))); then
  echo " $temp°C "
else
  echo " $temp°C "
fi
