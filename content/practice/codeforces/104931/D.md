---
title: "CF 104931D - Thế Giới Đảo Ngược"
description: "Chúng tôi được cung cấp một tập hợp các số mục tiêu riêng biệt. Chúng ta bắt đầu từ giá trị 1 và được phép xây dựng một chuỗi bằng cách nhân liên tục giá trị hiện tại với bất kỳ số nguyên dương nào."
date: "2026-06-28T07:35:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104931
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 1 (Advanced)"
rating: 0
weight: 104931
solve_time_s: 69
verified: false
draft: false
---

[CF 104931D - Thế giới đảo lộn](https://codeforces.com/problemset/problem/104931/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các số mục tiêu riêng biệt. Chúng ta bắt đầu từ giá trị 1 và được phép xây dựng một chuỗi bằng cách nhân liên tục giá trị hiện tại với bất kỳ số nguyên dương nào. Chuỗi luôn bắt đầu từ 1 và mỗi phần tử tiếp theo được lấy từ phần tử trước đó bằng một bước nhân. 

Câu hỏi không phải là xây dựng chuỗi đó mà là chọn nó theo cách tối đa hóa số lượng mục tiêu đã cho xuất hiện dưới dạng các phần tử của chuỗi. 

Được diễn đạt lại một cách có cấu trúc hơn, chúng ta đang tìm kiếm một chuỗi số bắt đầu từ 1 trong đó mỗi phần tử chia cho phần tử tiếp theo và mỗi lần chuyển đổi tương ứng với việc nhân với một số nguyên. Vì phép nhân với bất kỳ số nguyên dương nào đều được phép, hạn chế về cấu trúc duy nhất là mỗi bước sẽ tăng tính chia hết dọc theo chuỗi. Chúng ta muốn dãy con dài nhất của các số đã cho có thể được sắp xếp sao cho mỗi số chia hết cho số tiếp theo và dãy bắt đầu từ 1. 

Những hạn chế rất quan trọng. Chúng tôi có tới 1000 số, mỗi số có khả năng lớn tới 10^18. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng các cạnh giữa tất cả các cặp bằng hệ số nhân tử đắt tiền hoặc liệt kê tất cả các chuỗi nhân có thể có. Một giải pháp bậc hai trên các cặp đã có thể chấp nhận được ở đường biên, nhưng bất kỳ phép liệt kê bậc ba hoặc liên quan đến ước số cho mỗi cặp sẽ không thành công. 

Một trường hợp cạnh tinh tế phát sinh từ vai trò của 1. Vì chuỗi bắt đầu từ 1 nên mọi số bằng 1 trong đầu vào luôn tự động được đưa vào. Một trường hợp cạnh khác là khi các số nguyên tố cùng nhau theo cặp, ví dụ [2, 3, 5, 7]. Trong trường hợp đó, không thể xâu chuỗi vượt quá độ dài 1 và câu trả lời là 1, không phải tổng số. 

## Phương pháp tiếp cận 

Quan sát chính là cấu trúc hoạt động được phép thực thi chuỗi phân chia. Nếu chúng ta có một dãy 1 = x0, x1, x2, ..., xk, thì mỗi xi+1 phải là xi nhân với một số nguyên k, ngụ ý rằng xi chia hết cho xi+1. Ngược lại, bất kỳ chuỗi phân chia tăng dần nào bắt đầu từ 1 đều có thể được thực hiện bằng các số nhân thích hợp. 

Vì vậy, nhiệm vụ trở thành: trong số các số đã cho, tìm chuỗi dài nhất trong đó mỗi số chia cho số tiếp theo. 

Cách tiếp cận bạo lực sẽ cố gắng bắt đầu từ 1 và thử đệ quy tất cả các tập hợp con số, kiểm tra xem một ứng cử viên có thể mở rộng chuỗi hay không. Ở mỗi bước, chúng tôi sẽ thử tất cả các số còn lại và xác minh tính chia hết. Điều này dẫn đến số lượng trình tự tăng theo cấp số nhân và thậm chí với việc cắt tỉa, nó trở nên không khả thi đối với N lên tới 1000. 

Cái nhìn sâu sắc quan trọng là diễn giải lại vấn đề dưới dạng đường đi dài nhất trong biểu đồ tuần hoàn có hướng được xác định bởi khả năng chia hết. Mỗi số có thể chuyển thành bội số bất kỳ của chính nó trong danh sách. Nếu chúng ta sắp xếp các số theo thứ tự tăng dần thì bất kỳ chuỗi hợp lệ nào cũng phải tuân theo thứ tự này. Điều này biến bài toán thành một chuỗi tăng dài nhất theo quan hệ chia hết, có thể giải được bằng quy hoạch động. 

Chúng tôi xác định dp[i] là số lượng số yêu thích tối đa mà chúng tôi có thể đưa vào và kết thúc bằng a[i]. Chúng ta sắp xếp mảng và với mỗi cặp i < j sao cho a[j] % a[i] == 0, chúng ta có thể mở rộng dp[i] thành dp[j]. Câu trả lời là giá trị dp tối đa. 

Về cơ bản, đây là đường dẫn dài nhất trong DAG với các cạnh được xác định bằng khả năng chia hết và việc sắp xếp đảm bảo tính chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N * N) | O(N) | Quá chậm | 
| DP qua cặp chia hết | O(N^2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp các số đã cho theo thứ tự tăng dần. Điều này đảm bảo rằng nếu một số có thể chia hết cho số khác thì số liền trước tiềm năng sẽ xuất hiện sớm hơn, loại bỏ các chu kỳ và thực thi cấu trúc phụ thuộc chỉ chuyển tiếp. 
2. Khởi tạo một mảng dp trong đó dp[i] = 1 với mọi i. Mỗi số riêng lẻ tạo thành một chuỗi hợp lệ có độ dài 1. 
3. Lặp lại từng chỉ mục i từ trái sang phải. Với mỗi i, hãy xem xét tất cả các chỉ số trước đó j < i. Nếu a[j] chia a[i] thì chúng ta có thể mở rộng chuỗi kết thúc tại j bằng cách nối thêm a[i], vì vậy chúng ta cập nhật dp[i] = max(dp[i], dp[j] + 1). 
4. Theo dõi giá trị tối đa tính bằng dp trên tất cả các chỉ số. Điều này thể hiện chuỗi phân chia hợp lệ dài nhất trong số các số. 

Lý do chúng tôi chỉ kiểm tra j < i là vì việc sắp xếp đảm bảo tất cả các ước số có thể có của a[i] trong số đầu vào phải xuất hiện sớm hơn nếu chúng nhỏ hơn. 

### Tại sao nó hoạt động 

Bất biến chính là dp[i] luôn biểu thị độ dài tối đa của chuỗi hợp lệ kết thúc chính xác tại a[i] chỉ sử dụng các phần tử trong số i được sắp xếp đầu tiên. Mọi chuyển đổi đều bảo toàn tính hợp lệ vì tính chia hết đảm bảo rằng phép nhân với một số nguyên sẽ kết nối hai phần tử liên tiếp trong chuỗi. Vì mọi chuỗi hợp lệ phải tôn trọng tính chia hết và thứ tự tăng dần, nên mọi chuỗi có thể đều được biểu diễn dưới dạng một đường dẫn trong biểu đồ DP này và DP liệt kê tất cả các đường dẫn đó mà không lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    dp = [1] * n
    
    for i in range(n):
        for j in range(i):
            if a[i] % a[j] == 0:
                if dp[j] + 1 > dp[i]:
                    dp[i] = dp[j] + 1
    
    print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho tất cả các ước số tiềm năng của một số xuất hiện trước nó. Mảng dp được khởi tạo thành 1 vì mỗi số tự nó là một điểm cuối chuỗi hợp lệ. 

Vòng lặp lồng nhau là cốt lõi của giải pháp. Với mỗi cặp (j, i), chúng ta kiểm tra xem a[j] có chia hết a[i] hay không. Nếu đúng như vậy thì bất kỳ chuỗi nào kết thúc tại a[j] đều có thể được mở rộng thêm a[i], vì vậy chúng ta truyền bá giá trị tốt nhất được biết đến. Hoạt động tối đa đảm bảo chúng tôi chỉ giữ lại dây chuyền tốt nhất. 

Một lỗi phổ biến ở đây là đảo ngược việc kiểm tra tính chia hết hoặc quên sắp xếp. Nếu không sắp xếp, quá trình chuyển đổi dp có thể bỏ sót các chuyển đổi trước đó hợp lệ hoặc giả định không chính xác thứ tự không tồn tại. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
4
1 2 6 12
```Đầu tiên chúng tôi sắp xếp, mặc dù nó đã được sắp xếp. Chúng tôi tính toán dp từng bước. 

| tôi | một [tôi] | dp[i] | Cập nhật | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | bắt đầu | 
| 1 | 2 | 2 | 1 chia 2 | 
| 2 | 6 | 3 | 2 → 6 | 
| 3 | 12 | 4 | 6 → 12 | 

Đáp án cuối cùng là 4, tương ứng với chuỗi đầy đủ 1 → 2 → 6 → 12. 

Dấu vết này cho thấy tầm quan trọng của bội số trung gian: bỏ qua 2 sẽ giới hạn chuỗi ở độ dài 2, mặc dù 1 chia hết tất cả các số. 

Bây giờ hãy xem xét một trường hợp thưa thớt:```
4
2 3 5 7
```| tôi | một [tôi] | dp[i] | Cập nhật | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | không | 
| 1 | 3 | 1 | không | 
| 2 | 5 | 1 | không | 
| 3 | 7 | 1 | không | 

Không có số nào chia hết cho số khác nên chuỗi tốt nhất có độ dài bằng 1. 

Điều này xác nhận thuật toán xử lý chính xác các biểu đồ chia hết bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi cặp số được kiểm tra tính chia hết một lần | 
| Không gian | O(N) | mảng dp có kích thước N | 

Với N lên tới 1000, nghiệm bậc hai thực hiện tối đa 10^6 kiểm tra tính chia hết, nằm trong giới hạn thời gian. Mỗi lần kiểm tra là một phép toán modulo đơn trên số nguyên 64 bit, vì vậy nó hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    a.sort()
    dp = [1] * n
    
    for i in range(n):
        for j in range(i):
            if a[i] % a[j] == 0:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return str(max(dp))

# provided sample (interpreted)
assert run("3\n2 6 10 12\n") == "3"

# minimum size
assert run("1\n7\n") == "1"

# all equal divisibility chain
assert run("4\n1 1 1 1\n") == "4"

# coprime set
assert run("4\n2 3 5 7\n") == "1"

# full chain
assert run("4\n1 2 6 12\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 7 | 1 | trường hợp tối thiểu | 
| 1 1 1 1 | 4 | bản sao và chuỗi đầy đủ | 
| 2 3 5 7 | 1 | không có chuyển tiếp hợp lệ | 
| 1 2 6 12 | 4 | xích tối ưu | 

## Vỏ cạnh 

Đối với các đầu vào chứa nhiều số 1, chẳng hạn như:```
5
1 1 2 4 8
```sắp xếp giữ tất cả 1s đầu tiên. Mỗi 1 có thể mở rộng mọi chuỗi, vì vậy dp[0..k] đều trở thành những đóng góp ngày càng tăng. Thuật toán xử lý chính xác mọi số 1 như một ước số chung, tạo ra chuỗi dài nhất có thể kết thúc bằng số lớn nhất. 

Đối với trường hợp có số lượng lớn và không có trung gian:```
3
1 1000000000000000000 999999999999999999
```chỉ có 1 người có thể bắt đầu chuỗi. Cả số lớn đều không chia hết cho số kia, vì vậy dp vẫn bằng 1 cho cả hai. Kết quả là 2 nếu cả hai đều có thể truy cập độc lập dưới dạng điểm cuối, nhưng vì chúng tôi chỉ tính độ dài chuỗi nên điểm cuối tối đa vẫn là 1 trừ khi chuỗi được hình thành. DP ngăn chặn chính xác các bước nhảy không hợp lệ vì khả năng phân chia không thành công theo cả hai hướng.
