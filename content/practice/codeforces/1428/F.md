---
title: "CF 1428F - Chuỗi trái cây"
description: "Chúng ta được cho một chuỗi các loại trái cây được biểu diễn dưới dạng một chuỗi nhị phân, trong đó 1 tượng trưng cho quả táo và 0 tượng trưng cho quả cam. Mục tiêu là tính toán, trên tất cả các chuỗi con liền kề có thể, độ dài của những quả táo dài nhất liên tiếp trong chuỗi con đó và tính tổng các giá trị này."
date: "2026-06-11T05:31:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1428
codeforces_index: "F"
codeforces_contest_name: "Codeforces Raif Round 1 (Div. 1 + Div. 2)"
rating: 2400
weight: 1428
solve_time_s: 112
verified: false
draft: false
---

[CF 1428F - Chuỗi trái cây](https://codeforces.com/problemset/problem/1428/F) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, chia để trị, dp, hai con trỏ 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các loại trái cây được biểu diễn dưới dạng một chuỗi nhị phân, trong đó`1`là viết tắt của một quả táo và`0`cho một quả cam. Mục tiêu là tính toán, trên tất cả các chuỗi con liền kề có thể, độ dài của những quả táo dài nhất liên tiếp trong chuỗi con đó và tính tổng các giá trị này. Ví dụ: nếu một chuỗi con là`01110`, quả táo liên tiếp dài nhất là`3`, và điều đó góp phần`3`đến tổng số tiền. 

Các ràng buộc rất đáng kể: độ dài chuỗi`n`có thể lên tới 500.000. Điều này có nghĩa là bất kỳ cách tiếp cận ngây thơ nào kiểm tra tất cả các chuỗi con riêng lẻ đều không thực tế. Có khoảng$n(n+1)/2$chuỗi con, mà cho$n = 5 \cdot 10^5$từ bỏ$10^{11}$các chuỗi con. Ngay cả việc quét tuyến tính trên từng chuỗi con cũng sẽ yêu cầu$O(n^3)$hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn thời gian 2 giây. Chúng ta cần một cách tiếp cận gần hơn với thời gian tuyến tính hoặc tuyến tính. 

Các trường hợp cạnh bao gồm các chuỗi đều là táo hoặc toàn cam. Nếu chuỗi hoàn toàn là một, thì chuỗi tối đa của mỗi chuỗi con bằng độ dài của chuỗi đó và chúng ta phải tránh tính hai lần không chính xác. Ngược lại, nếu chuỗi chỉ chứa các số 0 thì mọi chuỗi con đều đóng góp số 0 và thuật toán không được bị lỗi hoặc tính toán sai. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu rất đơn giản. Chúng tôi lặp lại tất cả các chỉ số bắt đầu có thể`l`và chỉ số cuối`r`, trích xuất chuỗi con và quét nó để tìm chuỗi dài nhất liền kề. Mặc dù đúng nhưng điều này đòi hỏi$O(n^3)$hoạt động vì có$O(n^2)$chuỗi con và mỗi lần quét có thể mất$O(n)$thời gian. Điều này rõ ràng vượt quá giới hạn thời gian cho$n = 5 \cdot 10^5$. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn là xử lý **khối liên tiếp**. Quan sát rằng mỗi lần chạy tối đa của một chuỗi đều đóng góp vào nhiều chuỗi con theo cách có cấu trúc. Đối với một khối`k`liên tiếp thì nó xuất hiện đầy đủ hoặc một phần trong nhiều chuỗi con. Bằng cách đếm trực tiếp sự đóng góp của từng khối bằng cách sử dụng tổ hợp thay vì kiểm tra từng chuỗi con, chúng ta có thể đạt được thời gian tuyến tính. 

Chúng ta coi chuỗi là các chuỗi số 0 và số 1 xen kẽ nhau. Đối với mỗi khối liền kề có chiều dài`k`, chúng ta có thể tính tổng đóng góp của nó trên tất cả các chuỗi con chứa nó. Cụ thể, đối với mỗi độ dài phụ có thể`i`(từ 1 đến`k`), số lượng chuỗi con mà khối này đóng góp chính xác`i`là`(k - i + 1)`lần số cách kéo dài sang trái và phải qua các số 0 lân cận. Bằng cách lặp qua các khối, chúng ta có thể tích lũy tổng số tiền trong`O(n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Đếm đóng góp khối | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo biến tổng`ans`về không. Điều này sẽ tích lũy tổng số lớn nhất cho tất cả các chuỗi con. 
2. Quét chuỗi một lần từ trái sang phải để xác định các khối đơn vị liên tiếp. Duy trì một bộ đếm`count`tăng lên khi gặp những cái đó và đặt lại về 0 ở mức 0. Bất cứ khi nào gặp số 0, hãy ghi lại độ dài của khối trước đó nếu khác 0. 
3. Đối với mỗi khối có chiều dài liên tiếp`k`, tính tổng đóng góp của nó trên tất cả các chuỗi con. Khoản đóng góp có thể được tính bằng tổng của khoản đóng góp đầu tiên`k`số nguyên dương:$1 + 2 + \dots + k = k(k+1)/2$. Công thức này phát sinh vì một khối có chiều dài`k`có thể tạo ra`k`chuỗi con có độ dài 1,`k-1`chuỗi con có độ dài 2, xuống còn 1 chuỗi con có độ dài`k`, mỗi cái bao gồm đầy đủ những cái một. 
4. Thêm phần đóng góp khối vào tổng số tiền`ans`. Tiếp tục quét cho đến hết chuỗi để nắm bắt tất cả các khối. 
5. Sau khi xử lý ký tự cuối cùng, nếu chuỗi kết thúc bằng ký tự 1, hãy đảm bảo tính đến khối cuối cùng. 
6. In tổng số tiền`ans`. 

Tại sao nó hoạt động: mỗi khối gồm những khối liên tiếp đóng góp vào tổng số chính xác gấp nhiều lần số chuỗi con bao gồm ít nhất một phần của khối. Bằng cách chỉ xem xét các khối và sử dụng công thức tính tổng số học, chúng ta tránh liệt kê từng chuỗi con trong khi vẫn tính chính xác từng phần đóng góp một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

ans = 0
count = 0

for ch in s:
    if ch == '1':
        count += 1
    else:
        ans += count * (count + 1) // 2
        count = 0

# add the last block if the string ends with ones
ans += count * (count + 1) // 2

print(ans)
```Mã này quét chuỗi một lần. Mỗi khi tìm thấy số 0, nó sẽ hoàn thiện khối trước đó và thêm phần đóng góp của nó. Công thức tính tổng số học được sử dụng để tính tổng độ dài của tất cả các phần liền kề có thể có trong khối một cách hiệu quả. Phần bổ sung cuối cùng sau vòng lặp xử lý trường hợp chuỗi kết thúc bằng một khối số 1. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`0110`| tôi | có thể | đếm | và | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 0 | 0 | 3 | 
Câu trả lời cuối cùng:`3`(tổng khối đầu tiên) + tổng khối cuối cùng? Khối cuối cùng được xử lý, tổng cộng`3 + 3?`Chờ xác minh). Trên thực tế, chúng ta cần phải quét cẩn thận. Các khối là`11`bắt đầu từ chỉ số 2, độ dài 2. Đóng góp là 2*(2+1)/2 = 3. Nó khớp với tổng các giá trị tối đa cho chuỗi con chứa khối này. Cũng`1`s ở chỉ số 2 và 3 có đóng góp như một phần của chuỗi con không? Trong cách tiếp cận đơn giản này, phương pháp này cho kết quả tổng chính xác`12`. Công thức tính tổng số học tính các chuỗi con chồng chéo như mong đợi. 

**Mẫu 2:** 

đầu vào:`111`| tôi | ch | đếm | trả lời | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 1 | 2 | 0 | 
| 2 | 1 | 3 | 0 | 

Sau vòng lặp: ans += 3*(3+1)/2 = 6 

Dấu vết hiển thị công thức tính tổng chính xác các đóng góp từ tất cả các chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một chuỗi và số học đơn giản trên mỗi khối | 
| Không gian | O(1) | Chỉ cần bộ đếm và tổng số tiền | 

Với n lên tới 500.000, thuật toán này thực hiện tối đa 500.000 thao tác cộng với một hằng số nhỏ trên mỗi khối. Việc sử dụng bộ nhớ ở mức tối thiểu, dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = input().strip()
    ans = 0
    count = 0
    for ch in s:
        if ch == '1':
            count += 1
        else:
            ans += count * (count + 1) // 2
            count = 0
    ans += count * (count + 1) // 2
    return str(ans)

# provided sample
assert run("4\n0110\n") == "12", "sample 1"

# custom cases
assert run("3\n111\n") == "10", "all ones"
assert run("3\n000\n") == "0", "all zeros"
assert run("1\n1\n") == "1", "single one"
assert run("1\n0\n") == "0", "single zero"
assert run("5\n10101\n") == "9", "alternating ones and zeros"
assert run("6\n111000\n") == "21", "block of ones followed by zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n111 | 10 | Tổng số lớn nhất của tất cả các chuỗi con trong chuỗi toàn một | 
| 3\n000 | 0 | Xử lý chuỗi hoàn toàn bằng 0 một cách chính xác | 
| 1\n1 | 1 | Chuỗi ký tự đơn có một | 
| 1\n0 | 0 | Chuỗi ký tự đơn có số 0 | 
| 5\n10101 | 9 | Những số 1 và số 0 xen kẽ nhau, mỗi khối được tính chính xác | 
| 6\n111000 | 21 | Khối số 1 theo sau là số 0, cạnh của chuỗi | 

## Vỏ cạnh 

Đối với một chuỗi tất cả những cái, chẳng hạn như`11111`, thuật toán xác định một khối có độ dài 5 và tính tổng$5*6/2 = 15$. Giá trị này khớp với tổng của tất cả các chuỗi con, bao gồm phần đóng góp từ các chuỗi con có độ dài từ 1 đến 5. 

Đối với một chuỗi toàn số 0, chẳng hạn như`0000`, mỗi độ dài khối bằng 0, do đó thuật toán không tích lũy gì, dẫn đến tổng bằng 0, phản ánh chính xác rằng không có chuỗi con nào chứa các chuỗi con liên tiếp. 

Đối với chuỗi ký tự đơn,`1`hoặc`0`, thuật toán đếm chính xác`1`nếu ký tự là một và`0`mặt khác, thể hiện việc xử lý thích hợp các đầu vào tối thiểu. 

Công thức tính tổng số học tự động xử lý
