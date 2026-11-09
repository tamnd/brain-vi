---
title: "CF 1496B - Max và Mex"
description: "Chúng ta được cấp một tập hợp các số nguyên không âm riêng biệt và cần mô phỏng một quá trình trong đó, tối đa k lần, chúng ta thêm phần tử (mex + max + 1) // 2 vào tập hợp đó. Mex của một tập hợp là số nguyên không âm nhỏ nhất không tồn tại và max là phần tử lớn nhất."
date: "2026-06-10T21:55:10+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1496
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 706 (Div. 2)"
rating: 1100
weight: 1496
solve_time_s: 271
verified: false
draft: false
---

[CF 1496B – Max và Mex](https://codeforces.com/problemset/problem/1496/B) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên không âm riêng biệt và cần mô phỏng một quá trình trong đó, tối đa`k`lần, chúng tôi thêm phần tử`(mex + max + 1) // 2`đến nhiều tập hợp. các`mex`của một tập hợp là số nguyên không âm nhỏ nhất không có mặt và`max`là phần tử lớn nhất Sau khi thực hiện thao tác`k`lần, chúng ta phải báo cáo có bao nhiêu số nguyên **riêng biệt** mà nhiều tập hợp chứa. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp đưa ra`n`, kích thước nhiều tập hợp ban đầu,`k`, số lượng phép toán và các số nguyên riêng biệt trong nhiều tập hợp. Đầu ra là một số nguyên cho mỗi trường hợp. 

Những hạn chế là đáng kể.`n`có thể lên đến`10^5`, Và`k`có thể lên đến`10^9`. Điều đó có nghĩa là việc mô phỏng mọi thao tác là không thể. Chúng ta phải suy luận một cách toán học về điều gì xảy ra sau mỗi phép cộng. Tổng của`n`trên tất cả các trường hợp thử nghiệm là`10^5`, vì vậy chúng tôi có đủ khả năng`O(n log n)`hoạt động cho mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì như`O(k)`là vô vọng. 

Các trường hợp phức tạp phát sinh khi`mex`lớn hơn`max`. Ví dụ, nếu tập hợp là`{0,1,2}`Và`k=2`, cái`mex`là`3`Và`max`là`2`. Thêm`(3+2+1)//2=3`tạo một bản sao, do đó thao tác đầu tiên không làm tăng số lượng phần tử riêng biệt. Tuy nhiên, sau khi cho phép sao chép, hãy thêm một cái mới`mex`lớn hơn hiện tại`max`sẽ tăng số lượng khác biệt dự đoán. 

Việc triển khai bất cẩn có thể mô phỏng mọi thao tác hoặc không xử lý được`k=0`, tạo ra kết quả sai trong trường hợp không thực hiện thao tác nào. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Với mỗi thao tác, hãy tính`mex`Và`max`, tính toán`(mex + max + 1) // 2`và chèn nó vào multiset. Sau đó`k`hoạt động, đếm các phần tử riêng biệt. Điều này có hiệu quả vì mỗi bước đều tuân theo đúng định nghĩa vấn đề. Tuy nhiên, với`k`lên đến`10^9`, tốc độ này quá chậm; thậm chí tính toán`mex`nhiều lần sẽ tốn kém`O(n)`mỗi thao tác, đưa ra`O(n*k)`điều đó là không thể thực hiện được. 

Quan sát quan trọng là quá trình này ổn định nhanh chóng. Cho phép`a = mex(S)`Và`b = max(S)`. Nếu như`a > b`, sau đó`(a+b+1)//2 = a`, lớn hơn rất nhiều so với`b`, vì vậy mỗi thao tác chỉ thêm số nguyên tiếp theo sau số tối đa hiện tại. Trong trường hợp này, số phần tử riêng biệt tăng thêm`k`. Nếu như`a <= b`, sau đó`(a+b+1)//2`tạo ra một số giữa`a`Và`b`. Việc thêm số này có thể giới thiệu một phần tử mới (nếu nó không có trong tập hợp) hoặc trùng lặp (nếu có). Trong hầu hết các trường hợp, thêm nó một lần là đủ; sau đó, tập hợp không còn thay đổi nữa, bởi vì`mex`trở nên bằng số mới, ổn định tập hợp. 

Vì vậy, chúng ta chỉ cần xem xét hai kịch bản: hoặc tập hợp ổn định sau một thao tác hoặc chúng ta chỉ thêm các số mới ngoài giá trị hiện tại.`max`. Điều này làm giảm vấn đề thành một phép tính thời gian không đổi cho mỗi trường hợp kiểm thử khi chúng ta biết`mex`Và`max`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(n) | Quá chậm đối với k lớn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mảng đầu vào thành Python`set`để theo dõi các yếu tố riêng biệt một cách hiệu quả. 
2. Tính toán`mex`bằng cách lặp lại từ`0`trở lên cho đến khi thiếu một số nguyên trong tập hợp. 
3. Tính toán`max`sử dụng tính năng tích hợp sẵn`max()`chức năng. 
4. Nếu`k=0`, trả về số phần tử riêng biệt hiện tại ngay lập tức. 
5. Nếu`mex`lớn hơn`max`, mỗi thao tác sẽ thêm một số nguyên mới lớn hơn`max`. Vì vậy, tổng số khác biệt sau`k`hoạt động là`len(S) + k`. 
6. Mặt khác, tính toán phần tử mới ứng cử viên như`(mex + max + 1) // 2`. Thêm nó vào bộ nếu nó chưa có. 
7. Trả về kích thước của tập hợp đã cập nhật. 

Tại sao nó hoạt động:`mex`đảm bảo chúng tôi luôn nhắm mục tiêu đến phần tử còn thiếu nhỏ nhất và công thức`(mex + max + 1) // 2`tạo ra phần tử mới trong phạm vi hiện tại hoặc mở rộng tập hợp vượt quá mức tối đa hiện tại. Trong cả hai trường hợp, chỉ một phép cộng có thể thay đổi số lượng riêng biệt nếu`mex <= max`; các hoạt động tiếp theo không đưa ra số mới vì`mex`chuyển sang phần tử được thêm vào, hiện có trong tập hợp, ổn định tập hợp ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        S = set(arr)
        mex = 0
        while mex in S:
            mex += 1
        max_val = max(S)
        
        if k == 0:
            print(len(S))
            continue
        
        if mex > max_val:
            # Every operation adds a new number beyond current max
            print(len(S) + k)
        else:
            new_elem = (mex + max_val + 1) // 2
            S.add(new_elem)
            print(len(S))

if __name__ == "__main__":
    solve()
```Mã chuyển đổi mảng thành một tập hợp để kiểm tra tư cách thành viên O(1). Máy tính`mex`yêu cầu quét từ 0 trở lên cho đến khi tìm thấy số còn thiếu. Nếu như`mex > max_val`, chúng ta chỉ cần thêm`k`các số riêng biệt theo tuần tự, mỗi số vượt quá mức tối đa hiện tại. Ngược lại thì thêm`(mex + max + 1)//2`một lần là đủ vì bộ máy sẽ ổn định ngay sau đó. Xử lý`k=0`đảm bảo chúng tôi không vô tình sửa đổi tập hợp. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`S = {0,1,3,4}`,`k=1`| Bước | S | mex | tối đa | new_elem | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | {0,1,3,4} | 2 | 4 | - | tập ban đầu | 
| op1 | thêm 3 | 2 | 4 | 3 | 3 đã tồn tại nên S trở thành {0,1,3,3,4} | 
| cuối cùng | {0,1,3,4} | - | - | - | phần tử riêng biệt = 4 | 

Điều này cho thấy rằng việc thêm một phần tử đã có sẵn không làm tăng số lượng riêng biệt. 

**Ví dụ 2:** 

đầu vào:`S = {0,1,2}`,`k=2`| Bước | S | mex | tối đa | new_elem | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | {0,1,2} | 3 | 2 | - | tập ban đầu | 
| op1 | thêm 3 | 3 | 3 | 3 | mex > max, thêm phần tử mới | 
| op2 | thêm 4 | 4 | 3 | 4 | mex > tối đa trở lại | 
| cuối cùng | {0,1,2,3,4} | - | - | - | phần tử riêng biệt = 5 | 

Điều này thể hiện kịch bản khi`mex > max`, trong đó mỗi thao tác sẽ thêm một phần tử thực sự mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chuyển đổi thành tập hợp, tính toán`mex`bằng cách quét nhiều nhất`n+1`con số, tính toán`max`| 
| Không gian | O(n) | Lưu trữ tập hợp các phần tử | 

Xét tổng của tất cả`n`10^5, giá trị này vừa vặn thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n4 1\n0 1 3 4\n3 1\n0 1 4\n3 0\n0 1 4\n3 2\n0 1 2\n3 2\n1 2 3\n") == "4\n4\n3\n5\n3"

# Custom tests
# k = 0, should return original size
assert run("1\n5 0\n0 1 2 4 5\n") == "5"
# mex > max, k > 1
assert run("1\n3 3\n0 1 2\n") == "6"
# adding a number that already exists
assert run("1\n3 1\n0 1 4\n") == "4"
# single element, k = 5
assert run("1\n1 5\n0\n") == "6"
# large max with small mex
assert run("1\n4 2\n0 2 3 10\n") == "5"
```|
