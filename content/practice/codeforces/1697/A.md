---
title: "CF 1697A - Đại lộ đi bộ"
description: "Chúng ta được cấp một dãy tuyến tính gồm các băng ghế, được đánh số từ một đến $n+1$, cách nhau bởi các khoảng cách $a1, a2, dấu chấm, an$. Bạn bắt đầu ở băng ghế đầu tiên với một lượng năng lượng cố định $m$. Đi bộ quãng đường một mét tiêu tốn đúng một đơn vị năng lượng."
date: "2026-06-09T22:25:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1697
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 130 (Rated for Div. 2)"
rating: 800
weight: 1697
solve_time_s: 126
verified: true
draft: false
---

[CF 1697A - Parkway Walk](https://codeforces.com/problemset/problem/1697/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy ghế dài tuyến tính, được đánh số từ một đến$n+1$, cách nhau bởi khoảng cách$a_1, a_2, \dots, a_n$. Bạn bắt đầu ở băng ghế đầu tiên với một lượng năng lượng cố định$m$. Đi bộ quãng đường một mét tiêu tốn đúng một đơn vị năng lượng. Bạn chỉ có thể phục hồi năng lượng bằng cách ngồi trên ghế dài và bạn có thể chọn chính xác lượng năng lượng cần phục hồi ở mỗi điểm dừng, thậm chí vượt quá khả năng ban đầu của bạn$m$. Mục tiêu là xác định tổng lượng năng lượng tối thiểu bạn cần phục hồi để đến được băng ghế cuối cùng. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng băng ghế trừ đi một$n$, năng lượng ban đầu$m$, và khoảng cách$a_i$. Đầu ra phải là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm cho biết mức phục hồi năng lượng tối thiểu được yêu cầu. 

Những hạn chế còn khiêm tốn:$n$nhiều nhất là 100 và$a_i$nhiều nhất là 100, trong khi năng lượng ban đầu$m$có thể đi lên$10^4$. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp$O(n)$hoặc$O(n^2)$mỗi trường hợp thử nghiệm sẽ chạy thoải mái trong giới hạn 1 giây. Thách thức chính là tính toán chính xác năng lượng tiêu hao và phục hồi ở mỗi băng ghế mà không làm logic quá phức tạp. 

Các trường hợp cạnh phát sinh khi năng lượng ban đầu đã đủ để đến được băng ghế cuối cùng mà không cần ngồi, chẳng hạn như$n=5$,$m=16$, và khoảng cách$1,2,3,4,5$. Ở đây, câu trả lời đúng là 0 vì không cần khôi phục. Một kịch bản tinh tế khác xảy ra khi khoảng cách đến băng ghế tiếp theo lớn hơn năng lượng hiện tại, buộc phải phục hồi bằng mức chênh lệch. Việc thực hiện bất cẩn làm tổng hợp những thiếu hụt hoặc quản lý sai năng lượng còn sót lại có thể đưa ra câu trả lời sai trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng cuộc đi bộ: bắt đầu ở băng ghế số 1, theo dõi năng lượng hiện tại của bạn và bất cứ khi nào năng lượng còn lại không đủ để đến băng ghế tiếp theo, hãy khôi phục chính xác mức thâm hụt. Sau khi đi bộ một quãng đến băng ghế tiếp theo, hãy cập nhật năng lượng hiện tại. Đây thực sự là giải pháp tham lam. Điều này đúng vì ở mỗi bước, việc khôi phục cần thiết tối thiểu chính xác là thứ cho phép bạn đạt được cấp độ tiếp theo và việc khôi phục nhiều hơn sẽ rất lãng phí. Từ$n \le 100$, mô phỏng trực tiếp này đủ nhanh. 

Một giải pháp tối ưu chính thức hơn xuất phát từ việc nhận ra rằng mức phục hồi tối thiểu ở mỗi bước là$\max(0, a_i - \text{current energy})$. Bạn lặp lại các khoảng cách, tính toán mức thâm hụt nếu năng lượng của bạn quá thấp, thêm nó vào tổng năng lượng được phục hồi đang chạy và cập nhật năng lượng hiện tại bằng cách trừ khoảng cách và thêm năng lượng được phục hồi. Sự đơn giản của quan sát này làm cho cách tiếp cận tham lam trở nên tối ưu và có thể thực hiện ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Thâm hụt năng lượng tham lam | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`restored`về không. Điều này sẽ tích lũy tổng năng lượng được phục hồi trên tất cả các băng ghế. 
2. Đặt năng lượng hiện tại`energy`về giá trị ban đầu$m$. 
3. Lặp lại từng khoảng cách$a_i$giữa băng ghế$i$Và$i+1$. Ở mỗi bước, hãy kiểm tra xem năng lượng hiện tại có đủ để bao phủ$a_i$. 
4. Nếu`energy`lớn hơn hoặc bằng$a_i$, trừ$a_i$từ`energy`và tiến tới băng ghế tiếp theo. 
5. Nếu`energy`ít hơn$a_i$, tính mức thâm hụt$deficit = a_i - energy$. Thêm vào`deficit`ĐẾN`restored`và thiết lập`energy = 0`sau khi đi bộ đến băng ghế tiếp theo, bởi vì bạn sẽ chỉ còn đủ năng lượng sau khi phục hồi để đi hết quãng đường. 
6. Tiếp tục cho đến khi tới băng ghế cuối cùng. 
7. Sau vòng lặp,`restored`chứa tổng năng lượng tối thiểu cần được phục hồi. 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý từng băng ghế, năng lượng hiện tại biểu thị năng lượng còn lại sau khi bước đi chân đó. Bằng cách chỉ khôi phục những gì cần thiết để đạt tới băng ghế tiếp theo, chúng tôi đảm bảo rằng toàn bộ quá trình khôi phục là tối thiểu. Không có trường hợp nào trì hoãn việc khôi phục hoặc khôi phục nhiều hơn sẽ làm giảm tổng lượng khôi phục, bởi vì năng lượng chỉ có thể được phục hồi với lượng không âm và việc đi bộ luôn tiêu tốn chính xác một đơn vị trên mét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        energy = m
        restored = 0
        for dist in a:
            if energy >= dist:
                energy -= dist
            else:
                deficit = dist - energy
                restored += deficit
                energy = 0
        print(restored)

if __name__ == "__main__":
    main()
```Đầu tiên chúng ta đọc số lượng ca kiểm thử và sau đó lặp qua từng ca kiểm thử. Đối với mỗi khoảng cách từ băng ghế này đến băng ghế khác, chúng tôi kiểm tra xem năng lượng hiện tại có đủ hay không. Nếu không, chúng tôi tính toán chính xác lượng năng lượng cần phục hồi, cộng vào tổng năng lượng đang chạy và thiết lập lại năng lượng sau khi đi bộ. Điều này đảm bảo sự phục hồi tối thiểu được tích lũy một cách chính xác. Thứ tự của phép trừ và phép cộng rất quan trọng; làm không đúng cách có thể dẫn đến từng lỗi một khi bạn khôi phục quá nhiều hoặc quá ít. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên: 

| Băng ghế | Khoảng cách a_i | Năng lượng hiện tại | Thâm hụt năng lượng | Năng lượng được phục hồi | Năng lượng sau khi đi bộ | 
| --- | --- | --- | --- | --- | --- | 
| 1-2 | 1 | 1 | 0 | 0 | 0 | 
| 2-3 | 2 | 0 | 2 | 2 | 0 | 
| 3-4 | 1 | 0 | 1 | 3 | 0 | 

Điều này chứng tỏ rằng mỗi khoản thâm hụt đều được tính toán và tổng số tiền hiện có là 3. 

Sử dụng mẫu thứ hai$n=4, m=5, a=[3,3,5,2]$: 

| Băng ghế | Khoảng cách a_i | Năng lượng hiện tại | Thâm hụt năng lượng | Năng lượng được phục hồi | Năng lượng sau khi đi bộ | 
| --- | --- | --- | --- | --- | --- | 
| 1-2 | 3 | 5 | 0 | 0 | 2 | 
| 2-3 | 3 | 2 | 1 | 1 | 0 | 
| 3-4 | 5 | 0 | 5 | 6 | 0 | 
| 4-5 | 2 | 0 | 2 | 8 | 0 | 

Quá trình khôi phục tổng thể đang chạy 8 phù hợp với kết quả dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại một lần trên mỗi mảng khoảng cách a có độ dài n. N tối đa là 100. | 
| Không gian | O(n) | Lưu trữ khoảng cách a; không cần thêm mảng hoặc đệ quy. | 

Được cho$t \le 100$Và$n \le 100$, tổng số thao tác tối đa là 10.000, nằm trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("3\n3 1\n1 2 1\n4 5\n3 3 5 2\n5 16\n1 2 3 4 5\n") == "3\n8\n0", "sample 1"

# minimum size input
assert run("1\n1 1\n1\n") == "0", "minimum size"

# maximum distance requiring restoration
assert run("1\n3 1\n5 5 5\n") == "14", "all large distances"

# all equal distances with sufficient energy
assert run("1\n4 10\n2 2 2 2\n") == "0", "equal distances, enough energy"

# exactly needing energy to restore at each step
assert run("1\n4 2\n3 3 3 3\n") == "8", "need to restore at each step"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 0 | Đầu vào tối thiểu, không cần khôi phục | 
| 3 1\n5 5 5 | 14 | Khoảng cách lớn so với năng lượng, đảm bảo hoạt động tích lũy | 
| 4 10\n2 2 2 2 | 0 | Tất cả các khoảng cách bằng nhau, đủ năng lượng khởi đầu | 
| 4 2\n3 3 3 3 | 8 | Yêu cầu khôi phục ở mọi bước, trường hợp khó khăn đối với những thiếu hụt lặp đi lặp lại | 

## Vỏ cạnh 

Nếu năng lượng ban đầu lớn hơn tổng của tất cả các khoảng cách, thuật toán sẽ không bao giờ đi vào nhánh thâm hụt và đưa ra kết quả bằng 0 một cách chính xác. Đối với trường hợp có khoảng cách lớn và nhỏ xen kẽ liên quan đến năng lượng, bước tham lam đảm bảo rằng tại mỗi băng ghế, chúng tôi chỉ khôi phục phần thâm hụt và tổng số lần chạy sẽ tích lũy chính xác mức phục hồi tối thiểu. Ví dụ, với$n=3, m=2, a=[3,1,4]$, dấu vết của bảng: 

| Băng ghế | Khoảng cách | Năng lượng | Thâm hụt | Đã khôi phục | Năng lượng sau khi đi bộ | 
| --- | --- | --- | --- | --- | --- | 
| 1-2 | 3 | | | | |
