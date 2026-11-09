---
title: "CF 1486A - Ngăn xếp dịch chuyển"
description: "Chúng ta được cấp một chuỗi các ngăn xếp, mỗi ngăn xếp có một số khối. Nhiệm vụ là quyết định xem liệu chúng ta có thể phân phối lại các khối hay không, chỉ di chuyển chúng sang bên phải để làm cho chiều cao ngăn xếp tăng lên một cách nghiêm ngặt."
date: "2026-06-10T23:07:07+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1486
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 703 (Div. 2)"
rating: 900
weight: 1486
solve_time_s: 113
verified: true
draft: false
---

[CF 1486A - Ngăn xếp dịch chuyển](https://codeforces.com/problemset/problem/1486/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các ngăn xếp, mỗi ngăn xếp có một số khối. Nhiệm vụ là quyết định xem liệu chúng ta có thể phân phối lại các khối hay không, chỉ di chuyển chúng sang bên phải để làm cho chiều cao ngăn xếp tăng lên một cách nghiêm ngặt. Mỗi lần di chuyển bao gồm lấy một khối từ ngăn xếp$i$và đặt nó vào ngăn xếp$i+1$. Chúng tôi không loại bỏ hoặc loại bỏ ngăn xếp; ngay cả những ngăn xếp trống vẫn được giữ nguyên. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết số lượng ngăn xếp$n$và một danh sách các độ cao. Đầu ra là CÓ nếu chúng ta có thể đạt được trình tự tăng nghiêm ngặt và KHÔNG nếu ngược lại. 

Những hạn chế còn khiêm tốn:$n \le 100$và tổng của tất cả$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$10^4$. Điều này cho phép bất kỳ thuật toán nào gần như$O(n)$cho mỗi trường hợp thử nghiệm, vì$10^4$tổng số hoạt động có thể thực hiện được trong giới hạn thời gian 1 giây. Tuy nhiên, một lực lượng thô sơ mô phỏng các khối đơn lẻ chuyển động liên tục có thể mất tới$10^9$bước nếu chiều cao ngăn xếp gần giới hạn trên, tốc độ này quá chậm. 

Các trường hợp cạnh có thể gặp phải một giải pháp đơn giản bao gồm các ngăn xếp có chiều cao bằng 0, tất cả các ngăn xếp bằng nhau hoặc số lượng rất lớn yêu cầu tích lũy cẩn thận các khối dư thừa. Ví dụ: một đầu vào như`[0, 0, 1]`không thể thực hiện tăng dần vì hai ngăn xếp đầu tiên bắt đầu từ 0 và chúng ta không thể di chuyển các khối sang trái. Tương tự`[1000000000, 1000000000, 1000000000]`có thể được điều chỉnh bằng cách di chuyển dần dần các khối thừa sang bên phải để tạo thành`[0, 1, 2]`cộng với sự bù đắp, chứng tỏ rằng giải pháp phải theo dõi thặng dư tích lũy, không chỉ sự khác biệt của từng cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ mô phỏng mọi động thái có thể xảy ra, lấy từng khối một và dịch chuyển nó sang phải cho đến khi không thể thực hiện được thêm động thái nào nữa. Về nguyên tắc, điều này đúng vì cuối cùng tất cả các khối dư thừa sẽ được phân phối để thỏa mãn điều kiện tăng nghiêm ngặt nếu có thể. Vấn đề là một trường hợp thử nghiệm duy nhất có nhiều chiều cao$10^9$sẽ cần hàng tỷ bước di chuyển, điều này là không thể thực hiện được. 

Thông tin chi tiết quan trọng là cần lưu ý rằng mỗi ngăn xếp có thể đóng góp một lượng "dư thừa" nhất định cho ngăn xếp tiếp theo. Nếu ngăn xếp đầu tiên có nhiều hơn 0 khối, nó có thể cung cấp tối đa$h_0$khối sang ngăn xếp tiếp theo. Tổng quát hơn,$i$-thứ ngăn xếp phải có khả năng cung cấp đủ khối để đảm bảo rằng tất cả các vị trí trước đó có thể đạt được yêu cầu ngày càng tăng nghiêm ngặt. Chính thức, sau khi phân phối các khối thừa từ trái sang phải,$i$-ngăn xếp thứ phải có ít nhất$i$tổng số khối. Điều này biến vấn đề thành một lần quét tuyến tính đơn giản, theo dõi thặng dư tích lũy và kiểm tra xem mỗi ngăn xếp có thể đáp ứng mức tối thiểu cần thiết hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng của tất cả các độ cao) | O(n) | Quá chậm đối với độ cao lớn | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng chiều cao$h$. 
2. Khởi tạo một biến`surplus`về không. Điều này sẽ theo dõi số khối chúng ta có thể chuyển sang ngăn xếp tiếp theo. 
3. Lặp lại các ngăn xếp từ trái sang phải bằng chỉ mục$i$(dựa trên 0). 
4. Đối với mỗi ngăn xếp, hãy thêm chiều cao hiện tại$h[i]$ĐẾN`surplus`. Điều này thể hiện tất cả các khối có sẵn để sử dụng, bao gồm cả những gì đã được chuyển qua. 
5. Kiểm tra xem`surplus`ít nhất là$i$, số khối tối thiểu cần thiết cho đến nay để làm cho dãy tăng dần. Nếu như`surplus < i`, xuất NO và dừng đối với trường hợp thử nghiệm này. 
6. Ngược lại thì trừ$i$từ`surplus`tính đến các khối được sử dụng để đạt được yêu cầu ngày càng tăng nghiêm ngặt ở vị trí này. Tiếp tục đến ngăn xếp tiếp theo. 
7. Nếu tất cả các ngăn xếp đều đáp ứng yêu cầu, xuất ra CÓ. 

Lý do điều này có hiệu quả là`surplus`theo dõi tổng số khối chúng ta có thể dịch chuyển sang phải. Yêu cầu ngày càng khắt khe về chỉ số$i$là phải có ít nhất$i$các khối được tích lũy tại thời điểm đó, bởi vì trình tự tăng dần tối thiểu cho chỉ số$i$là`[0, 1, 2, ..., n-1]`. Nếu chúng ta có thể đáp ứng yêu cầu này ở mỗi bước thì việc phân phối cuối cùng luôn có thể được thực hiện bằng cách dịch chuyển các khối sang phải, làm cho chuỗi tăng lên một cách chặt chẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))
        surplus = 0
        possible = True
        for i in range(n):
            surplus += h[i]
            if surplus < i:
                possible = False
                break
            surplus -= i
        print("YES" if possible else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`.`surplus`tích lũy các khối có sẵn. Tại mỗi ngăn xếp, chúng tôi đảm bảo có đủ khối để thỏa mãn điều kiện tăng nghiêm ngặt. Nếu không, chúng ta kết thúc sớm để tránh tính toán không cần thiết. Trừ`i`từ`surplus`tính đến việc sử dụng các khối để đáp ứng yêu cầu tối thiểu tại vị trí đó. Các lỗi xảy ra có thể tránh được bằng cách sử dụng lập chỉ mục dựa trên 0 một cách nhất quán. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`[3, 4, 4, 4]`: 

| tôi | h[i] | dư trước khi kiểm tra | yêu cầu tôi | dư sau khi kiểm tra | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 0+4=4 | 0 | 4-0=4 | 
| 1 | 4 | 4+4=8 | 1 | 8-1=7 | 
| 2 | 4 | 7+4=11 | 2 | 11-2=9 | 

Tất cả các ngăn xếp đều đáp ứng yêu cầu, đầu ra là CÓ. 

đầu vào`[0, 1, 0]`: 

| tôi | h[i] | dư trước khi kiểm tra | yêu cầu tôi | dư sau khi kiểm tra | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 0+1=1 | 1 | 1-1=0 | 
| 2 | 0 | 0+0=0 | 2 | 0<2 → KHÔNG | 

Điều này cho thấy thuật toán phát hiện chính xác các khối không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ngăn xếp được xử lý chính xác một lần bằng phép tính đơn giản | 
| Không gian | O(1) thêm | Chỉ một số số nguyên được sử dụng; mảng đầu vào chiếm ưu thế trong bộ nhớ, dù sao thì điều này cũng được yêu cầu | 

Cho tổng của$n \le 10^4$trong tất cả các trường hợp thử nghiệm, giải pháp này chạy thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("6\n2\n1 2\n2\n1 0\n3\n4 4 4\n2\n0 0\n3\n0 1 0\n4\n1000000000 1000000000 1000000000 1000000000\n") == \
"YES\nYES\nYES\nNO\nNO\nYES"

# custom cases
assert run("1\n1\n0\n") == "YES", "single stack zero"
assert run("1\n1\n100\n") == "YES", "single stack non-zero"
assert run("1\n3\n0 0 0\n") == "NO", "all zeros"
assert run("1\n3\n0 1 2\n") == "YES", "already strictly increasing"
assert run("1\n3\n2 2 2\n") == "YES", "equal heights that can be adjusted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0\n`| CÓ | Chiều cao bằng không của ngăn xếp đơn | 
|`1\n1\n100\n`| CÓ | Chiều cao ngăn xếp đơn khác 0 | 
|`1\n3\n0 0 0\n`| KHÔNG | Tất cả số không, không thể tăng | 
|`1\n3\n0 1 2\n`| CÓ | Đã tăng nghiêm ngặt | 
|`1\n3\n2 2 2\n`| CÓ | Chiều cao bằng nhau, có thể phân phối lại | 

## Vỏ cạnh 

cho`[0, 0, 1]`, thuật toán tính toán`surplus=0`ở chỉ số 0,`surplus=0`ở chỉ số 1, và`surplus=1`tại chỉ số 2. Mức tối thiểu bắt buộc là 2 tại chỉ mục 2. Vì`surplus < i`, nó xuất ra KHÔNG. Điều này xử lý chính xác trường hợp hai ngăn xếp đầu tiên trống và không có cách nào để dịch chuyển các khối sang trái. 

Vì`[1000000000, 1000000000, 1000000000]`,`surplus`phát triển nhanh chóng: 1e9 → 2e9-1 → 3e9-3. Ở mỗi bước,`surplus >= i`,
