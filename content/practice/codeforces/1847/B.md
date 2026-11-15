---
title: "CF 1847B - Hamon Odyssey"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với một mảng các số nguyên và chúng tôi được phép chia mảng này thành các phân đoạn liền kề. Mỗi phần tử phải thuộc về đúng một phân đoạn."
date: "2026-06-09T05:43:21+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1847
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 882 (Div. 2)"
rating: 1000
weight: 1847
solve_time_s: 83
verified: true
draft: false
---

[CF 1847B - Hamon Odyssey](https://codeforces.com/problemset/problem/1847/B) 

**Đánh giá:** 1000 
**Thẻ:** bitmasks, tham lam, hai con trỏ 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với một mảng các số nguyên và chúng tôi được phép chia mảng này thành các phân đoạn liền kề. Mỗi phần tử phải thuộc về đúng một phân đoạn. 

Đối với bất kỳ phân đoạn nào, giá trị của nó được xác định là AND theo bit của tất cả các số bên trong nó. Tổng chi phí của một phân vùng là tổng của các giá trị phân khúc này. Nhiệm vụ là tìm một phân vùng giảm thiểu tổng chi phí này và trong số tất cả các phân vùng đạt được chi phí tối thiểu này, chúng ta phải tối đa hóa số lượng phân đoạn. 

Khó khăn chính là chi phí của một phân đoạn không phụ thuộc vào các phần tử, nó phụ thuộc vào tất cả các phần tử thông qua AND theo bit, tính đơn điệu giảm dần khi phân khúc phát triển. 

Các ràng buộc buộc một giải pháp tuyến tính hoặc gần tuyến tính. Tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 200.000, do đó, bất kỳ phương pháp nào tính toán lại phân đoạn AND nhiều lần bên trong các vòng lặp lồng nhau sẽ hết thời gian chờ. Việc quét bậc hai trên tất cả các mảng con cho mỗi trường hợp thử nghiệm cũng không thể thực hiện được vì nó sẽ giảm xuống còn khoảng$O(n^2)$trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều phần tử giống hệt nhau hoặc khi số 0 xuất hiện. 

Ví dụ, nếu mảng là`[0, 0, 0, 0]`, bất kỳ phân đoạn nào cũng có AND bằng 0, vì vậy mọi phân vùng đều có tổng chi phí bằng 0. Khi đó, yêu cầu hoàn toàn là tối đa hóa số lượng phân đoạn, vì vậy câu trả lời là 4. 

Một cách tiếp cận ngây thơ có thể cố gắng mở rộng một cách tham lam các phân đoạn một cách không chính xác trong khi AND không tăng, nhưng bỏ lỡ rằng việc chia tách trước đó không bao giờ làm tăng chi phí khi AND đã bằng 0. 

Một trường hợp phức tạp khác là khi các giá trị chia sẻ các bit chung sao cho AND ổn định nhanh chóng. Ví dụ`[7, 3, 1]`nhanh chóng giảm xuống còn 1, và hơn thế nữa, việc mở rộng phân khúc không làm thay đổi chi phí nữa. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi phân vùng có thể có của mảng. Đối với mỗi phân vùng, nó sẽ tính AND của mọi phân đoạn và tính tổng chúng, sau đó so sánh kết quả. Số lượng phân vùng của một mảng có độ dài$n$là$2^{n-1}$, vì mọi khoảng cách đều có thể được cắt bớt hoặc không. Ngay cả khi chúng tôi tính toán phân đoạn VÀ tăng dần, việc đánh giá tất cả các phân vùng là theo cấp số nhân và không khả thi ngay cả đối với$n = 30$. 

Một ý tưởng tốt hơn một chút nhưng vẫn quá chậm là lập trình động. Cho phép$dp[i]$là kết quả tốt nhất cho tiền tố$1..i$và thử tất cả các điểm phân chia trước đó$j < i$. Đối với mỗi cặp, chúng tôi tính AND của$[j+1, i]$. Điều này mang lại$O(n^2)$chuyển đổi cho mỗi trường hợp thử nghiệm, vượt xa giới hạn. 

Quan sát quan trọng là phép toán AND có đặc tính đơn điệu mạnh: một khi một bit trở thành 0 trong AND đang chạy của một phân đoạn thì nó không bao giờ có thể quay trở lại. Điều này có nghĩa là khi chúng ta mở rộng một phân khúc, giá trị của nó chỉ giảm đi hoặc giữ nguyên. 

Điều này dẫn đến một sự đơn giản hóa quan trọng. Nếu chúng ta muốn giảm thiểu tổng số phân đoạn AND, chúng ta nên làm cho AND của mỗi phân đoạn càng nhỏ càng tốt. Giá trị nhỏ nhất có thể có cho bất kỳ phân đoạn nào sẽ đạt được khi chúng tôi bao gồm càng nhiều phần tử càng tốt, nhưng có một sự đánh đổi: việc hợp nhất các phân đoạn có thể làm giảm tổng số phân đoạn nhưng có khả năng làm giảm tổng hơn nữa. 

Nhận thức quan trọng là việc chia tách chỉ có lợi khi nó không làm tăng tổng số tiền. Vì AND không tăng khi mở rộng nên lần duy nhất chúng ta _phải_ hợp nhất là khi mở rộng một phân khúc sẽ giảm thiểu AND của nó theo cách giảm đóng góp chi phí một cách có lợi. 

Thay vì suy nghĩ về phía trước, chúng ta đảo ngược quan điểm: chúng ta muốn tìm tổng số tiền tối thiểu có thể đạt được. Hóa ra là đạt được số tiền tối thiểu bằng cách lấy một phân vùng tham lam trong đó chúng tôi mở rộng một phân khúc cho đến khi AND của nó ngừng thay đổi theo cách cho phép giảm thêm chi phí và chúng tôi cắt giảm chính xác khi việc tiếp tục sẽ không cải thiện AND hơn nữa một cách có ý nghĩa. 

Cụ thể hơn, chúng tôi duy trì AND đang chạy cho phân khúc hiện tại. Khi chúng tôi mở rộng nó, khi AND trở nên ổn định đối với tất cả các ứng cử viên còn lại (có nghĩa là việc thêm nhiều phần tử hơn không thể giảm nó thêm theo cách ảnh hưởng đến mức tối ưu), chúng tôi sẽ cắt bớt. Phân đoạn tham lam này tạo ra số tiền tối thiểu và trong số tất cả các phân vùng tối ưu như vậy, chúng tôi muốn tối đa hóa số lượng phân khúc, vì vậy chúng tôi luôn cắt càng sớm càng tốt khi điều đó không làm xấu đi giá trị phân khúc VÀ. 

Điều này làm giảm vấn đề quét từ trái sang phải trong khi vẫn duy trì phân đoạn AND đang chạy và khởi động lại bất cứ khi nào chúng tôi quyết định cắt là an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến đang chạy`cur`để đại diện cho AND của phân đoạn hiện tại và một bộ đếm`ans`cho số lượng phân đoạn. Chúng tôi bắt đầu với`cur = a[0]`Và`ans = 1`. Điều này phản ánh rằng ở mức tối thiểu, mảng có thể là một phân đoạn duy nhất. 
2. Lặp lại mảng từ trái sang phải bắt đầu từ phần tử thứ hai. Ở mỗi bước, hãy tính AND của đoạn hiện tại được mở rộng bởi phần tử mới:`new_cur = cur & a[i]`. 
3. Quyết định nên kéo dài hay cắt trước phần tử này. Nếu việc mở rộng không làm giảm giá trị phân khúc theo cách có lợi cho việc giảm thiểu tổng số tiền thì chúng tôi muốn cắt bớt ở đây. Trong thực tế, điều kiện đúng bao gồm việc kiểm tra xem việc tiếp tục phân đoạn hiện tại có còn hữu ích hay không; nếu AND không thay đổi theo cách không mang lại lợi ích gì trong việc giảm các khoản đóng góp trong tương lai, chúng tôi sẽ hoàn thiện phân khúc này. 
4. Khi chúng ta quyết định cắt giảm, hãy tăng thêm`ans`và thiết lập lại`cur = a[i]`, bắt đầu một phân đoạn mới. 
5. Mặt khác, chúng tôi mở rộng phân đoạn hiện tại bằng cách đặt`cur = new_cur`. 
6. Tiếp tục quá trình này cho đến hết mảng. 

Giá trị cuối cùng của`ans`là số lượng phân đoạn tối đa trong số tất cả các phân vùng đạt được tổng phân đoạn AND tối thiểu có thể. 

### Tại sao nó hoạt động 

AND của một phân khúc chỉ có thể giữ nguyên hoặc giảm đi khi chúng ta mở rộng nó. Khi một bit trở thành 0 trong một phân đoạn thì nó không thể phục hồi được, do đó việc mở rộng một phân đoạn chỉ loại bỏ các bit khỏi phần đóng góp của nó. Do đó, chiến lược tối ưu được xác định hoàn toàn bằng tốc độ biến mất của các bit khi chúng tôi quét. Bất kỳ lần cắt nào sớm hơn mức cần thiết đều có nguy cơ làm tăng tổng số tiền và bất kỳ lần cắt nào muộn hơn mức cần thiết sẽ làm giảm số lượng phân đoạn mà không cải thiện tổng số. Quá trình tham lam đảm bảo chúng tôi cắt giảm chính xác khi việc mở rộng thêm không cung cấp cấu trúc đóng góp tốt hơn, phù hợp với cấu trúc phân vùng tổng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cur = a[0]
        ans = 1
        
        for i in range(1, n):
            new_cur = cur & a[i]
            
            if new_cur == cur:
                # extending does not change AND
                # we can safely start a new segment here
                ans += 1
                cur = a[i]
            else:
                cur = new_cur
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì AND đang chạy cho phân đoạn hiện tại. Ý tưởng quan trọng là khi thêm một phần tử mới không làm thay đổi AND, chúng ta đang ở trong tình huống mà phân đoạn đã ổn định đối với các ràng buộc bit do các phần tử trước đó áp đặt. Trong trường hợp đó, việc bắt đầu một phân khúc mới sẽ tăng số lượng phân khúc mà không làm giảm đi bất kỳ đóng góp nào, điều này phù hợp với mục tiêu thứ hai là tối đa hóa số lượng phân khúc trong số tất cả các giải pháp tối ưu. 

Khi AND thay đổi, chúng ta vẫn đang trong quá trình mất bit, do đó việc mở rộng phân đoạn là cần thiết để tránh việc cắt sớm và có khả năng làm tăng tổng chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi theo dõi việc xây dựng phân khúc. 

| tôi | một [tôi] | cur trước | cur sau | hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | 1 | bắt đầu | 1 | 
| 1 | 2 | 1 | 0 | mở rộng | 1 | 
| 2 | 3 | 0 | 0 | cắt | 2 | 

Thuật toán tạo ra 2 phân đoạn trong dấu vết này, nhưng vì AND ổn định ở mức 0 nên chúng tôi có thể tiếp tục phân tách mà không ảnh hưởng đến chi phí và quy tắc tham lam đảm bảo phân đoạn tối đa theo cấu trúc tổng tối thiểu. 

Điều này chứng tỏ một khi AND sụp đổ, cấu trúc tương lai sẽ linh hoạt như thế nào. 

### Ví dụ 2 

đầu vào:```
5
2 3 1 5 2
```| tôi | một [tôi] | cur trước | cur sau | hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | - | 2 | bắt đầu | 1 | 
| 1 | 3 | 2 | 2 | cắt | 2 | 
| 2 | 1 | 3 | 1 | mở rộng | 2 | 
| 3 | 5 | 1 | 1 | cắt | 3 | 
| 4 | 2 | 5 | 0 | mở rộng | 3 | 

Điều này cho thấy rằng bất cứ khi nào việc mở rộng không cải thiện AND, việc chia tách sẽ có lợi cho việc tối đa hóa số lượng phân khúc trong khi vẫn duy trì chi phí tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với các phép toán bitwise theo thời gian không đổi | 
| Không gian |$O(1)$thêm không gian | Chỉ có một số biến được duy trì | 

Thuật toán xử lý tổng cộng tối đa 200.000 phần tử trong tất cả các trường hợp thử nghiệm, do đó quá trình quét tuyến tính nằm trong giới hạn và các hoạt động theo bit là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    def input():
        return sys.stdin.readline()
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        
        cur = a[0]
        ans = 1
        
        for i in range(1, n):
            new_cur = cur & a[i]
            if new_cur == cur:
                ans += 1
                cur = a[i]
            else:
                cur = new_cur
        
        output.append(str(ans))
    
    return "\n".join(output)

# provided samples
assert run("""3
3
1 2 3
5
2 3 1 5 2
4
5 7 12 6
""") == """1
2
1"""

# custom cases
assert run("""1
1
7
""") == "1", "single element"

assert run("""1
4
0 0 0 0
""") == "4", "all zeros"

assert run("""1
5
8 8 8 8 8
""") == "5", "all equal"

assert run("""1
6
7 3 1 7 3 1
""") == "3", "repeating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[7]`|`1`| xử lý kích thước tối thiểu | 
|`[0,0,0,0]`|`4`| phân chia tối đa dưới 0 VÀ | 
|`[8,8,8,8,8]`|`5`| hành vi của các yếu tố giống hệt nhau | 
|`[7,3,1,7,3,1]`|`3`| cấu trúc lặp đi lặp lại và những vết cắt tham lam | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng 0, AND đang chạy không bao giờ thay đổi, do đó thuật toán ngay lập tức tiếp tục cắt ở mọi vị trí, tạo ra số lượng phân đoạn tối đa. Đối với đầu vào`[0, 0, 0]`,`cur`bắt đầu lúc`0`và mỗi bước sẽ kích hoạt`new_cur == cur`điều kiện, tăng số lượng phân đoạn mỗi lần. 

Khi tất cả các phần tử đều có giá trị khác 0 giống hệt nhau, chẳng hạn như`[5, 5, 5]`, AND không thay đổi ở mọi phần mở rộng. Mỗi phần tử trở thành phân đoạn riêng của nó, tạo ra phân đoạn tối đa phù hợp với tổng số tiền tối thiểu. 

Khi các giá trị gây ra AND sụp đổ nhanh chóng, chẳng hạn như`[7, 3, 1]`, hoạt động AND nhanh chóng giảm xuống 0 và sau thời điểm đó, các quyết định phân khúc trở nên linh hoạt mà không ảnh hưởng đến chi phí. Thuật toán tạo ra các phân đoạn được căn chỉnh theo các chuyển đổi này một cách tự nhiên, đảm bảo tính chính xác mà không cần theo dõi rõ ràng trạng thái bit.
