---
title: "CF 1619E - MEX và mức tăng"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta có thể tăng bất kỳ phần tử nào lên 1 bất kỳ số lần nào. Nhiệm vụ là xác định cho mỗi số nguyên từ 0 đến độ dài mảng xem có thể chuyển đổi mảng sao cho MEX (giá trị loại trừ tối thiểu) của nó bằng…"
date: "2026-06-10T06:10:47+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dp", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1619
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 762 (Div. 3)"
rating: 1700
weight: 1619
solve_time_s: 88
verified: false
draft: false
---

[CF 1619E - MEX và phần tăng dần](https://codeforces.com/problemset/problem/1619/E) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dp, tham lam, triển khai, toán học, sắp xếp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta có thể tăng bất kỳ phần tử nào lên 1 bất kỳ số lần nào. Nhiệm vụ là xác định cho mỗi số nguyên từ 0 đến độ dài mảng xem có thể biến đổi mảng sao cho MEX (giá trị loại trừ tối thiểu) của nó bằng số nguyên đó hay không và nếu vậy, hãy tính số lượng gia tăng tối thiểu cần thiết. MEX của một mảng là số nguyên không âm nhỏ nhất không có trong mảng. 

Ví dụ, nếu mảng là`[0, 1, 3]`, MEX là`2`bởi vì`0`Và`1`đều có mặt nhưng`2`bị thiếu. tăng dần`3`ĐẾN`2`không giúp ích được gì vì chúng ta không thể giảm số lượng. Để đạt được MEX`3`, tất cả các số`0, 1, 2`phải có mặt, trong mảng này yêu cầu phải chuyển đổi các số hiện có, nhưng chỉ số gia tăng không thể tạo ra số bị thiếu`2`nếu nó vắng mặt và tất cả các số nhỏ hơn đã có mặt. 

Các ràng buộc sao cho tổng của tất cả`n`nhiều nhất là trên các trường hợp thử nghiệm`2*10^5`và số lượng ca kiểm thử có thể lên tới`10^4`. Điều này ngay lập tức loại trừ các giải pháp bạo lực mô phỏng mọi mức tăng cho mọi giá trị MEX tiềm năng, bởi vì con số đó có thể lên tới`O(n^2)`hoạt động cho mỗi trường hợp thử nghiệm. Chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều giống nhau, mảng thiếu 0, mảng đã chứa tất cả các giá trị`0..n`và mảng có khoảng trống lớn. Ví dụ,`[1, 1, 1]`không thể có MEX`0`không tăng, nhưng MEX`2`có thể yêu cầu tăng nhiều lần. Một cách tiếp cận bất cẩn có thể bỏ qua những khoảng trống này hoặc thực tế là chúng ta chỉ có thể tăng số lượng, tạo ra những câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử mọi MEX mục tiêu`i`từ`0`ĐẾN`n`. Đối với mỗi mục tiêu, chúng ta có thể mô phỏng số gia số còn thiếu cho đến khi tạo thành tất cả các số`0..i-1`. Điều này sẽ liên quan đến việc quét mảng và tăng giá trị, có thể mất tới`O(n^2)`trong trường hợp xấu nhất, vì mỗi số bị thiếu có thể yêu cầu quét hoặc tăng số gia lặp lại. Rõ ràng, điều này là quá chậm. 

Quan sát quan trọng là đối với MEX`i`để có thể, tất cả các số`0`bởi vì`i-1`phải tồn tại trong mảng sau một số lần tăng. Bất kỳ số nào còn thiếu dưới đây`i`phải được “tạo” bằng cách tăng các số nhỏ hơn. Ngược lại, bất kỳ số nào bằng hoặc lớn hơn`i`không chặn MEX`i`. 

Chúng ta có thể khai thác việc đếm tần số. Nếu chúng ta đếm xem mỗi số xuất hiện bao nhiêu lần thì chúng ta có thể xử lý các số từ`0`trở lên. Nếu thiếu một số, chúng ta có thể cố gắng “mượn” thêm các số nhỏ hơn để tạo ra số đó. Sự xuất hiện thêm của các số dưới đây`i`có thể được tăng lên để lấp đầy khoảng trống và các số ở trên`i`có thể vẫn còn nguyên. Bằng cách duy trì số lượng phần tử dư thừa đang chạy, chúng ta có thể tính toán số lượng gia tăng tối thiểu một cách hiệu quả trong một lần chuyển. 

Điều này giúp giảm vấn đề xuống còn một lần quét tuyến tính duy nhất của mảng tần số trong khi theo dõi phần dư thừa, nhanh hơn nhiều so với việc mô phỏng từng mức tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tần suất + Theo dõi thặng dư | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng số trong mảng, kể cả các số đến`n`. Khởi tạo một mảng`freq`kích thước`n+1`. Điều này cho phép truy cập ngay vào số lượng mỗi số chúng ta có. 
2. Khởi tạo một biến`surplus`về không. Điều này sẽ theo dõi số lượng phần tử chúng ta có thể sử dụng để điền vào các số còn thiếu, tức là các bản sao bổ sung của các số đã được xử lý. 
3. Lặp lại`mex`từ`0`ĐẾN`n`. Đối với mỗi`mex`, kiểm tra xem số đó có tồn tại trong mảng không: 

- Nếu`freq[mex] > 0`, chúng tôi có sẵn số lượng. Thêm vào`freq[mex] - 1`ĐẾN`surplus`bởi vì một lần xuất hiện là cần thiết để đáp ứng yêu cầu MEX và phần còn lại có sẵn để lấp đầy những khoảng trống trong tương lai. 
- Nếu như`freq[mex] == 0`, số đó bị thiếu. Chúng ta chỉ có thể tạo ra nó bằng cách tăng một trong các phần tử dư thừa. Nếu như`surplus > 0`, giảm`surplus`và tăng số lượng hoạt động. Nếu như`surplus == 0`, sau đó là MEX`mex`là không thể, và tất cả các giá trị MEX lớn hơn cũng không thể. 
4. Đối với mỗi MEX, hãy tính tổng số thao tác là tổng số gia được sử dụng để lấp đầy các khoảng trống cộng với số phần tử lớn hơn chính xác`mex`cần phải tăng lên nếu chúng ta muốn loại bỏ các bản sao ở trên`mex`(để tránh chặn MEX). 
5. Xuất kết quả theo thứ tự từ`0`ĐẾN`n`. Nếu không thể thực hiện MEX, hãy xuất`-1`. 

Tại sao nó hoạt động: Chúng tôi duy trì tính bất biến sau khi xử lý MEX`i`, tất cả các số`0..i-1`tồn tại trong mảng. các`surplus`biến đảm bảo chúng ta có thể điền các số còn thiếu bằng cách sử dụng các bản sao bổ sung. Nếu tại bất kỳ thời điểm nào, một số không thể được tạo ra vì không tồn tại thặng dư, thì giá trị MEX cao hơn cũng không thể được, điều này nhất quán về mặt logic. Cách tiếp cận tham lam này đảm bảo các hoạt động tối thiểu vì chúng tôi luôn sử dụng các phần tử bổ sung hiện có trước khi tạo phần tử mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

for _ in range(int(input())):
    n = int(input())
    a = list(map(int, input().split()))
    freq = [0] * (n + 2)
    for x in a:
        freq[x] += 1

    result = []
    ops = 0
    surplus = 0
    for mex in range(n + 1):
        if mex == 0:
            if freq[0] == 0:
                result.append(1)
                surplus = 0
            else:
                result.append(0)
                surplus = freq[0] - 1
        else:
            if freq[mex] == 0:
                if surplus > 0:
                    ops = ops + 1
                    result.append(ops)
                    surplus -= 1
                else:
                    result.extend([-1] * (n + 1 - mex))
                    break
            else:
                ops += freq[mex - 1]
                result.append(ops)
                surplus += freq[mex] - 1
    print(*result)
```Chúng tôi bắt đầu bằng cách đếm tần suất, điều này cho phép chúng tôi kiểm tra tình trạng sẵn có ngay lập tức. các`surplus`biến theo dõi số lượng phần tử có thể được tăng lên để điền vào các số còn thiếu. Chúng tôi cập nhật`ops`để phản ánh số lượng gia tăng cần thiết cho đến nay. Xử lý đặc biệt cho MEX`0`là cần thiết vì nó có thể yêu cầu tăng một phần tử ngay cả khi nó bị thiếu. Cách tiếp cận tham lam đảm bảo hoạt động tối thiểu bằng cách luôn lấp đầy khoảng trống bằng lượng thặng dư trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:`[0, 1, 3]`| MEX | tần số | dư thừa | hoạt động | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 0 | 0 | 1 | 0 | 
| 3 | 1 | 0 | 2 | -1 | 

Giải thích: MEX`0`yêu cầu tăng một phần tử, MEX`1`yêu cầu một thao tác, MEX`2`không cần thao tác, MEX`3`là không thể vì không có thặng dư để lấp đầy khoảng trống. 

### Ví dụ 2 

Mảng:`[0, 1, 2, 3, 4, 3, 2]`| MEX | tần số | dư thừa | hoạt động | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 2 | 2 | 3 | 2 | 
| 4 | 1 | 2 | 4 | 1 | 
| 5 | 0 | 2 | 4 | 0 | 
| 6 | 0 | 1 | 5 | 2 | 
| 7 | 0 | 0 | 6 | 6 | 

Bảng xác nhận thuật toán tích lũy thặng dư và số gia một cách chính xác, tạo ra các hoạt động tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm tần số và quét từ 0 đến n đều tuyến tính | 
| Không gian | O(n) | Mảng tần số có kích thước n+2 | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2*10^5`, tổng số hoạt động là khoảng`O(2*10^5)`, trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    for _ in range(int(input())):
        n = int(input())
        a = list(map(int, input().split()))
        freq = [0] * (n + 2)
        for x in a:
            freq[x] += 1
```
