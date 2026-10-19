---
title: "CF 105687B - Trò chơi bài"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các giá trị thẻ và tham số $k$."
date: "2026-06-22T05:00:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105687
codeforces_index: "B"
codeforces_contest_name: "AlgoChief Sprint Round 2"
rating: 0
weight: 105687
solve_time_s: 43
verified: true
draft: false
---

[CF 105687B - Trò chơi bài](https://codeforces.com/problemset/problem/105687/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có một mảng các giá trị thẻ và một tham số$k$. Từ những thẻ này, chúng ta được phép chọn một số dãy con, nhưng ràng buộc thứ tự thực tế hóa ra lại không liên quan một khi chúng ta diễn giải những gì lựa chọn đang cố gắng tối ưu hóa. Hạn chế cốt lõi là lựa chọn được chọn phải tôn trọng một cách hiệu quả cấu trúc nhóm gắn liền với$k$, nghĩa là cấu trúc hữu ích là có bao nhiêu nhóm kích thước đầy đủ$k$chúng ta có thể hình thành và giá trị chúng ta thu được từ chúng. 

Nhiệm vụ là tối đa hóa tổng số điểm thu được từ các thẻ đã chọn và điểm đơn giản hóa ẩn giấu chính là vị trí của các thẻ trong mảng ban đầu không hạn chế tính tối ưu ngoài số lượng chúng tôi chọn. Điều quan trọng chỉ là các giá trị chúng tôi đưa vào và thực tế là việc lựa chọn được xử lý hiệu quả theo từng khối kích thước$k$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra một số nguyên$n$, một số nguyên$k$, và một mảng$n$số nguyên đại diện cho giá trị thẻ. Đầu ra của mỗi trường hợp kiểm thử là một số nguyên duy nhất biểu thị số điểm tối đa có thể đạt được theo quy tắc lựa chọn. 

Từ cuộc thảo luận về các ràng buộc, ý nghĩa quan trọng là$n$có thể lớn trong mọi trường hợp thử nghiệm, có khả năng lên tới$10^5$và có thể có nhiều trường hợp thử nghiệm. Một DP ngây thơ phụ thuộc vào cả hai$n$Và$k$mỗi trường hợp thử nghiệm dẫn đến độ phức tạp khoảng$O(t \cdot n \cdot k)$, suy biến về khoảng$10^9$hoạt động trong trường hợp xấu nhất. Điều đó đã vượt quá giới hạn thông thường, do đó, bất kỳ giải pháp nào duy trì trạng thái hai chiều đối với các vị trí và số lượng nhóm sẽ không vượt qua. 

Một cạm bẫy tinh vi xuất hiện khi người ta giả định việc lựa chọn chuỗi con duy trì mức độ liên quan của thứ tự. Ví dụ, hãy xem xét các giá trị$[1, 100, 2, 99]$với$k = 2$. Một cách tiếp cận bất cẩn có thể cố gắng mô phỏng việc chọn các cặp tôn trọng các chỉ số, nhưng vì chỉ có tổng đóng góp mới quan trọng nên giải pháp tốt nhất chỉ đơn giản là chọn các giá trị cao nhất mà không cần lo lắng về vị trí kề hoặc vị trí ban đầu. Bất kỳ thuật toán nào bảo toàn không chính xác các ràng buộc thứ tự ban đầu sẽ tính thiếu hoặc làm phức tạp quá mức việc lựa chọn. 

## Phương pháp tiếp cận 

Công thức trực tiếp nhất là suy nghĩ về mặt lập trình động trên mảng và số lượng mục chúng ta đã lấy cho đến nay. Người ta có thể định nghĩa một trạng thái như$dp[i][j]$, nghĩa là chúng ta đang ở chỉ mục$i$và đã chọn$j$các phần tử, cố gắng tối đa hóa tổng theo quy tắc chỉ hoàn thành các nhóm có kích thước$k$đóng góp. Quá trình chuyển đổi sẽ xem xét việc lấy hoặc bỏ qua từng phần tử. 

Cách tiếp cận này đúng vì nó tôn trọng tất cả các ràng buộc một cách rõ ràng, nhưng nó sẽ trở nên tốn kém ngay lập tức. Số lượng các trạng thái là$O(n \cdot n)$theo cách giải thích tồi tệ nhất, hoặc ít nhất$O(n \cdot k)$và mỗi lần chuyển đổi là không đổi, dẫn đến hành vi bậc hai hoặc giả bậc hai cho mỗi trường hợp thử nghiệm. Với kích thước đầu vào lớn, điều này nhanh chóng trở nên không khả thi. 

Quan sát cấu trúc quan trọng là sự liền kề trong mảng ban đầu không ảnh hưởng đến điểm số cuối cùng. Vì việc lựa chọn là một dãy con nên chúng ta có thể tự do sắp xếp lại suy nghĩ của mình: điều quan trọng chỉ là giá trị nào được chọn chứ không phải chúng đến từ đâu. Điều này biến vấn đề thành một tối ưu hóa hoàn toàn dựa trên giá trị. 

Khi thứ tự không còn phù hợp, chiến lược tự nhiên là sắp xếp mảng theo thứ tự giảm dần. Sau khi sắp xếp, giải pháp tối ưu phải lấy các giá trị sẵn có lớn nhất trước tiên, bởi vì bất kỳ giá trị nhỏ hơn nào cũng không bao giờ có thể cải thiện một nhóm có thể được hình thành với giá trị lớn hơn thay vào đó. Vì sự đóng góp gắn liền với việc lấy các phần tử theo khối kích thước$k$, chúng tôi đánh giá tổng tiền tố của mảng được sắp xếp này và chỉ xem xét các vị trí trong đó số phần tử được chọn là bội số của$k$. Trong số các tiền tố hợp lệ này, tổng tiền tố tối đa là câu trả lời. 

Điều này hiệu quả vì mọi giải pháp hợp lệ đều tương ứng với việc chọn một số phần tử và đối với bất kỳ số lượng cố định nào, việc chọn các phần tử có sẵn lớn nhất sẽ tối đa hóa tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP qua các chỉ số và lượt chọn |$O(n^2)$hoặc$O(nk)$|$O(nk)$| Quá chậm | 
| Sắp xếp + đánh giá tiền tố |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị của một test case và lưu trữ chúng trong một mảng. Điều này cho phép chúng ta hoàn toàn tự do bỏ qua các vị trí ban đầu vì không có ràng buộc nào phụ thuộc vào tính kề sau khi diễn giải lại. 
2. Sắp xếp mảng theo thứ tự giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng tôi xem xét việc sử dụng các phần tử, chúng tôi luôn ưu tiên những phần tử có giá trị nhất trước tiên. 
3. Tính tổng tiền tố trên mảng đã được sắp xếp. Tổng tiền tố tại vị trí$i$đại diện cho tổng số tốt nhất có thể nếu chúng ta lấy chính xác phần trên cùng$i$các phần tử. 
4. Chỉ xem xét các chỉ số$i$như vậy$i$là bội số của$k$. Các vị trí này tương ứng với việc hình thành các nhóm kích thước hoàn chỉnh$k$, đây là những điểm kiểm tra có ý nghĩa duy nhất cho cơ cấu tính điểm. 
5. Theo dõi tổng tiền tố tối đa trong số các vị trí hợp lệ này và xuất ra dưới dạng câu trả lời. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp hợp lệ nào đều tương ứng với việc chọn một số tập hợp con có kích thước$m$và giá trị của nó là tổng của các phần tử được chọn. Đối với một cố định$m$, tập con tối ưu luôn là$m$phần tử lớn nhất vì thay thế phần tử được chọn nhỏ hơn bằng phần tử lớn hơn không được chọn sẽ làm tăng tổng. Vì ràng buộc nhóm chỉ phụ thuộc vào$m \bmod k = 0$, chúng tôi chỉ đánh giá bội số của$k$. Tiền tố được sắp xếp đảm bảo chúng tôi luôn đánh giá tập hợp con tốt nhất có thể cho từng kích thước, do đó, việc chọn tập hợp con tốt nhất trong số các ứng cử viên đó sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        prefix = 0
        best = 0
        
        for i in range(n):
            prefix += a[i]
            if (i + 1) % k == 0:
                best = max(best, prefix)
        
        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng theo thứ tự giảm dần sao cho mỗi tiền tố thể hiện lựa chọn tốt nhất có thể có cho một kích thước nhất định. Biến tổng tiền tố tích lũy tổng số thẻ đã chọn đang chạy. 

Chi tiết thực hiện quan trọng là kiểm tra`(i + 1) % k == 0`. Vì Python sử dụng tính năng lập chỉ mục dựa trên số 0 nên tiền tố có độ dài$i+1$hoàn thành khi nó tạo thành bội số của$k$. Chỉ tại những điểm đó, ràng buộc nhóm mới cho phép chúng tôi đánh giá cấu hình hợp lệ. 

Biến`best`theo dõi tổng tối đa trong số tất cả độ dài tiền tố hợp lệ. Chúng tôi khởi tạo nó về 0 vì không lấy phần tử nào luôn là đường cơ sở hợp lệ nếu có thể có giá trị âm hoặc nếu các ràng buộc cho phép lựa chọn trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 2
3 1 4 2 5
```Mảng được sắp xếp là$[5, 4, 3, 2, 1]$. 

| tôi | tiền tố đã chọn | tổng tiền tố | hợp lệ (i+1)%k==0 | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | [5] | 5 | không | 0 | 
| 1 | [5,4] | 9 | vâng | 9 | 
| 2 | [5,4,3] | 12 | không | 9 | 
| 3 | [5,4,3,2] | 14 | vâng | 14 | 
| 4 | [5,4,3,2,1] | 15 | không | 14 | 

Câu trả lời đúng nhất là 14, đến từ việc lấy 4 phần tử tốt nhất vì 4 là bội số lớn nhất của$k=2$. 

### Ví dụ 2 

đầu vào:```
1
6 3
10 10 1 2 3 4
```Mảng được sắp xếp là$[10, 10, 4, 3, 2, 1]$. 

| tôi | tiền tố đã chọn | tổng tiền tố | hợp lệ | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | [10] | 10 | không | 0 | 
| 1 | [10,10] | 20 | không | 0 | 
| 2 | [10,10,4] | 24 | vâng | 24 | 
| 3 | [10,10,4,3] | 27 | không | 24 | 
| 4 | [10,10,4,3,2] | 29 | không | 24 | 
| 5 | [10,10,4,3,2,1] | 30 | vâng | 30 | 

Câu trả lời tối ưu là 30, đạt được bằng cách lấy cả 6 phần tử vì 6 chia hết cho 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, quét tiền tố là tuyến tính | 
| Không gian |$O(n)$| Lưu trữ cho mảng và tích lũy tiền tố | 

Sự phức tạp phù hợp một cách thoải mái trong những ràng buộc điển hình ngay cả khi$n$lớn trên nhiều trường hợp thử nghiệm. Sắp xếp là bước tốn kém duy nhất và mọi thứ khác đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        prefix = 0
        best = 0
        for i in range(n):
            prefix += a[i]
            if (i + 1) % k == 0:
                best = max(best, prefix)
        out.append(str(best))
    return "\n".join(out)

# sample and custom tests
assert run("1\n1 1\n5\n") == "5"
assert run("1\n4 2\n1 2 3 4\n") == "10"
assert run("1\n5 2\n3 1 4 2 5\n") == "14"
assert run("1\n6 3\n10 10 1 2 3 4\n") == "30"
assert run("1\n3 2\n-1 -2 -3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp 1 phần tử | 5 | độ chính xác kích thước tối thiểu | 
| 4 phần tử, k=2 | 10 | lựa chọn đầy đủ đơn giản | 
| mẫu 1 | 14 | tiền tố đa logic | 
| mẫu 2 | 30 | trường hợp sử dụng đầy đủ | 
| tất cả đều tiêu cực | 0 | xử lý các giá trị âm và lựa chọn trống | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị đều âm. Ví dụ, hãy xem xét:```
1
3 2
-1 -2 -3
```Sau khi sắp xếp, chúng tôi nhận được$[-1, -2, -3]$. Tổng tiền tố là$-1, -3, -6$. Chỉ tiền tố có độ dài 2 là hợp lệ vì$2$là bội số duy nhất của$k$nhỏ hơn hoặc bằng$n$, và tổng của nó là$-3$. Tuy nhiên, việc chọn không có gì có thể được cho phép ngầm tùy theo cách giải thích, do đó thuật toán sẽ khởi tạo`best = 0`, đảm bảo câu trả lời không bị phủ định. 

Một trường hợp cạnh khác là khi$n < k$. Trong trường hợp này, không có tiền tố hợp lệ nào tồn tại ngoại trừ lựa chọn trống, do đó kết quả vẫn là 0. Việc sắp xếp và quét xử lý điều này một cách tự nhiên vì không có chỉ mục nào thỏa mãn`(i+1) % k == 0`, rời đi`best`không thay đổi.
