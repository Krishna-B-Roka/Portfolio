#!/bin/bash

echo "================================="
echo "       Student Grade Analyzer"
echo "================================="

file="students.txt"

if [ ! -f "$file" ]; then
    echo "Error: $file does not exist."
    exit 1
fi

while true
do
    echo ""
    read -p "Enter the minimum grade (0-100): " grade

    if ! [[ "$grade" =~ ^[0-9]+$ ]] || [ "$grade" -lt 0 ] || [ "$grade" -gt 100 ]; then
        echo "Please enter a number between 0 and 100."
        continue
    fi

    echo ""
    echo "1. Display the results"
    echo "2. Save the results to a file"

    read -p "Choose an option (1 or 2): " choice

    if [ "$choice" = "1" ]; then

        echo ""
        echo "Students with grade $grade or higher:"
        echo "---------------------------------"

        awk -v min="$grade" '$2 >= min {print $1, $2}' "$file"

    elif [ "$choice" = "2" ]; then

        output="results.txt"

        awk -v min="$grade" '$2 >= min {print $1, $2}' "$file" > "$output"

        echo "Results saved to $output"

    else
        echo "Invalid option."
    fi

    echo ""
    read -p "Do you want to search again? (y/n): " again

    if [ "$again" != "y" ] && [ "$again" != "Y" ]; then
        echo "Program finished. Goodbye!"
        break
    fi
done
