---
title: "CF 1175B - Bắt Tràn!"
description: "Chúng tôi có một chương trình đơn giản được viết bằng một ngôn ngữ nhỏ với ba lệnh: add, tăng biến x lên 1; đối với n, bắt đầu một vòng lặp lặp lại các lệnh bên trong nó n lần; và kết thúc, để đóng một vòng lặp."
date: "2026-06-12T01:47:55+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1175
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 66 (Rated for Div. 2)"
rating: 1600
weight: 1175
solve_time_s: 84
verified: true
draft: false
---

[CF 1175B - Bắt tràn!](https://codeforces.com/problemset/problem/1175/B) 

**Đánh giá:** 1600 
**Tags:** cấu trúc dữ liệu, phân tích biểu thức, triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chương trình đơn giản được viết bằng một ngôn ngữ nhỏ với ba lệnh:`add`, làm tăng một biến`x`bằng 1;`for n`, bắt đầu một vòng lặp lặp lại các lệnh bên trong nó`n`lần; Và`end`, để đóng một vòng lặp. Giá trị ban đầu của`x`là 0 và chúng tôi muốn tính giá trị cuối cùng của`x`sau khi chạy chương trình, nhưng`x`là số nguyên không dấu 32 bit, vì vậy nếu tại bất kỳ điểm nào nó vượt quá`2^32-1`, chương trình sẽ báo cáo`OVERFLOW!!!`. 

Đầu vào là chuỗi lệnh trong chương trình. Mỗi`for`có thể lồng nhau tùy ý nhưng tổng số dòng có thể lên tới`10^5`. Mỗi vòng lặp có thể được lặp lại tới 100 lần. Mục tiêu là mô phỏng việc thực thi một cách hiệu quả mà không thực sự thực hiện hàng tỷ phép cộng, bởi vì mô phỏng đơn giản có thể dễ dàng yêu cầu tới`10^5 * 100^l`hoạt động trong các vòng lặp lồng nhau sâu. 

Các trường hợp cạnh bao gồm một`add`bên ngoài bất kỳ vòng lặp nào, các vòng lặp có 1 lần lặp, các vòng lặp trống (ngay lập tức`for n`theo sau là`end`) và các tình huống trong đó các lần bổ sung lặp lại vượt quá giới hạn 32 bit. Ví dụ: nếu một vòng lặp nhân số lượng`add`vượt xa`2^32-1`, một bộ đếm ngây thơ có thể âm thầm bao quanh hoặc vượt quá số nguyên tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là mô phỏng trực tiếp từng lệnh, duy trì bộ đếm cho`x`. Mọi`add`sự gia tăng`x`bằng 1 và mọi`for n`sẽ chỉ đơn giản là thực thi khối bên trong`n`lần đệ quy. Điều này đúng nhưng quá chậm. Hãy xem xét một chương trình với`10^5`dòng, mỗi dòng là`for 100`với các vòng lặp lồng nhau bên trong: tổng số lần bổ sung có thể vượt quá`10^2000`, điều đó hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là theo dõi **hệ số nhân** cho các vòng lặp lồng nhau thay vì lặp lại mọi phép cộng. Mỗi`add`bên trong`k`các vòng lặp lồng nhau sẽ đóng góp`product_of_loop_counts`ĐẾN`x`. Chúng ta có thể duy trì một ngăn xếp để giữ hệ số nhân hiện tại ở mỗi cấp độ lồng nhau. Bất cứ khi nào chúng tôi nhập một vòng lặp, chúng tôi sẽ đẩy hệ số mới (hệ số hiện tại nhân với số vòng lặp) vào ngăn xếp. Khi chúng ta thoát khỏi một vòng lặp, chúng ta sẽ bật ngăn xếp. Chúng ta cũng cần ngừng nhân các yếu tố khi chúng vượt quá`2^32-1`, bởi vì bất kỳ sự bổ sung nào ngoài điều này đều được đảm bảo tràn. Điều này cho phép chúng ta mô phỏng chương trình theo thời gian tuyến tính tương ứng với số dòng. 

Cách tiếp cận này tránh được tổng trung gian lớn, giữ độ phức tạp về thời gian ở mức O(l) và trực tiếp phát hiện tràn mà không cần thực hiện hàng tỷ thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số bổ sung) | O(l) | Quá chậm | 
| Ngăn xếp yếu tố | O(l) | O(l) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`x`về 0 và một ngăn xếp có một thừa số`1`, đại diện cho số nhân của`add`s ở cấp độ lồng nhau hiện tại. 
2. Đối với mỗi dòng trong chương trình, hãy phân tích lệnh. Nếu lệnh là`add`, tăng`x`ở đầu ngăn xếp, bởi vì điều này thể hiện số lần hiệu quả`add`được lặp đi lặp lại do các vòng lặp kèm theo. Nếu như`x`vượt quá`2^32-1`, báo tràn ngay. 
3. Nếu lệnh là`for n`, tính hệ số mới như`current_factor * n`. Nếu sản phẩm này vượt quá`2^32-1`, Đẩy một giá trị trọng điểm đại diện cho vô cực (hoặc một giá trị lớn hơn giới hạn) để chỉ ra rằng bất kỳ`add`trong phạm vi này sẽ gây tràn. Ngược lại, đẩy hệ số mới vào ngăn xếp. 
4. Nếu lệnh là`end`, bật ngăn xếp để xóa phạm vi hiện tại và quay lại hệ số nhân trước đó. 
5. Sau khi xử lý tất cả các dòng, nếu không xảy ra tràn, hãy in`x`. 

Lý do điều này có tác dụng là vì ngăn xếp luôn biểu diễn hệ số nhân chính xác cho`add`s ở cấp độ lồng nhau hiện tại. Bằng cách giới hạn các hệ số ở ngưỡng tràn, chúng tôi tránh được số học quá mức và đảm bảo rằng bất kỳ tình trạng tràn nào có thể xảy ra đều được phát hiện trước khi nó xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 2**32 - 1

def main():
    l = int(input())
    stack = [1]  # current multiplier at each nesting level
    x = 0
    overflow = False
    
    for _ in range(l):
        line = input().strip()
        if line == "add":
            x += stack[-1]
            if x > MAX:
                overflow = True
                break
        elif line.startswith("for"):
            n = int(line.split()[1])
            if stack[-1] > MAX // n:
                stack.append(MAX + 1)  # sentinel for guaranteed overflow
            else:
                stack.append(stack[-1] * n)
        elif line == "end":
            stack.pop()
    
    print("OVERFLOW!!!" if overflow else x)

if __name__ == "__main__":
    main()
```Mã tuân theo thuật toán một cách chính xác. Ngăn xếp duy trì hệ số nhân và kiểm tra`stack[-1] > MAX // n`ngăn phép nhân số nguyên vượt quá 2^32-1. Giá trị canh gác đảm bảo bất kỳ sự bổ sung nào trong ngữ cảnh tràn sẽ kích hoạt phát hiện tràn. Ngăn xếp bật lên`end`khôi phục chính xác yếu tố trước đó. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
9
add
for 43
end
for 10
for 15
add
end
add
end
```| Dòng | Lệnh | Ngăn xếp | x | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | thêm | [1] | 1 |`add`vòng lặp bên ngoài | 
| 2 | cho 43 | [1, 43] | 1 | nhập vòng lặp 43 lần lặp | 
| 3 | kết thúc | [1] | 1 | vòng lặp trống, không thêm | 
| 4 | cho 10 | [1, 10] | 1 | nhập vòng lặp 10x | 
| 5 | cho 15 | [1, 10, 150] | 1 | 10*15=150 hệ số nhân | 
| 6 | thêm | [1, 10, 150] | 151 | 1 + 150 thêm | 
| 7 | kết thúc | [1, 10] | 151 | thoát khỏi vòng lặp bên trong | 
| 8 | thêm | [1, 10] | 161 | 151 + 10 (vòng ngoài) | 
| 9 | kết thúc | [1] | 161 | thoát khỏi vòng lặp bên ngoài | 

Cuối cùng`x`= 161, không tràn. 

**Mẫu 2 (Tràn)** 

đầu vào:```
2
for 100000
add
end
```| Dòng | Lệnh | Ngăn xếp | x | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | với giá 100000 | [1, 100000] | 0 | nhập vòng lặp | 
| 2 | thêm | [1, 100000] | 100000 | phép cộng nhân với hệ số | 
| 3 | kết thúc | [1] | 100000 | thoát vòng lặp | 

Ngay cả ví dụ nhỏ này cũng an toàn, nhưng nếu các vòng lặp lồng nhau nhân hệ số vượt quá 2^32-1, thì trọng điểm sẽ kích hoạt tính năng phát hiện tràn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(l) | Mỗi dòng được xử lý một lần, các thao tác ngăn xếp là O(1) | 
| Không gian | O(l) | Độ sâu ngăn xếp có thể có nhiều nhất số vòng lặp lồng nhau, giới hạn bởi l | 

Được cho`l <= 10^5`, thuật toán tuyến tính này thực thi thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("9\nadd\nfor 43\nend\nfor 10\nfor 15\nadd\nend\nadd\nend\n") == "161", "sample 1"
assert run("2\nfor 4294967296\nadd\nend\n") == "OVERFLOW!!!", "overflow case"

# Custom cases
assert run("1\nadd\n") == "1", "single add"
assert run("3\nfor 1\nadd\nend\n") == "1", "loop of 1"
assert run("5\nfor 2\nfor 2\nadd\nend\nend\n") == "4", "nested loops"
assert run("4\nfor 4294967295\nadd\nadd\nend\n") == "OVERFLOW!!!", "overflow by double add"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\nadd`| 1 | vòng lặp ngoài cộng đơn | 
|`3\nfor 1\nadd\nend`| 1 | vòng lặp 1 lần được xử lý chính xác | 
|`5\nfor 2\nfor 2\nadd\nend\nend`| 4 | phép nhân vòng lặp lồng nhau | 
|`4\nfor 4294967295\nadd\nadd\nend`| TRÀN!!! | ngoài ra còn gây tràn vượt quá giới hạn | 

## Vỏ cạnh 

Một vòng lặp ngay sau đó`end`thực thi số không`add`s, do đó thuật toán đẩy và bật hệ số một cách chính xác nhưng không bao giờ tăng `x
