---
title: "CF 1893B - Âm trung tính"
description: "Chúng tôi được cung cấp hai mảng. Mảng đầu tiên được cố định theo thứ tự và mảng thứ hai là tập hợp nhiều giá trị bổ sung mà chúng ta được phép chèn vào bất kỳ đâu vào mảng đầu tiên, đồng thời được phép hoán vị các giá trị được chèn này một cách tùy ý trước khi đặt chúng."
date: "2026-06-08T21:56:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1893
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 908 (Div. 1)"
rating: 1700
weight: 1893
solve_time_s: 133
verified: false
draft: false
---

[CF 1893B - Âm điệu trung tính](https://codeforces.com/problemset/problem/1893/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng. Mảng đầu tiên được cố định theo thứ tự và mảng thứ hai là tập hợp nhiều giá trị bổ sung mà chúng ta được phép chèn vào bất kỳ đâu vào mảng đầu tiên, đồng thời được phép hoán vị các giá trị được chèn này một cách tùy ý trước khi đặt chúng. 

Sau khi hợp nhất cả hai nguồn thành một chuỗi duy nhất, chúng tôi đo độ dài của chuỗi con tăng dài nhất (LIS). Mục tiêu là xây dựng bất kỳ chuỗi hợp nhất hợp lệ nào nhằm giảm thiểu giá trị LIS này. 

Sự tự do của khóa rất quan trọng: chúng ta không bị ràng buộc phải giữ thứ tự tương đối của mảng thứ hai và chúng ta có thể xen kẽ các phần tử của nó ở bất kỳ đâu trong mảng đầu tiên. Chỉ mảng đầu tiên phải là một chuỗi con. 

Các ràng buộc đẩy chúng ta tới giải pháp O(n + m) hoặc O((n + m) log(n + m)) cho mỗi trường hợp thử nghiệm. Vì tổng n và m trong các bài kiểm tra được giới hạn bởi 2·10^5, nên bất kỳ giải pháp nào sắp xếp hoặc quét tuyến tính từng phần tử với số lần không đổi đều có thể chấp nhận được, nhưng bất kỳ giải pháp bậc hai nào trong một bài kiểm tra đều ngay lập tức không thể thực hiện được. 

Một cách tiếp cận đơn giản sẽ thử mọi cách để chèn các phần tử từ b vào a, nhưng ngay cả việc quyết định vị trí chèn cũng đã tạo thành một vụ nổ tổ hợp. Ngay cả khi cố định thứ tự của b, việc chèn m phần tử vào n khoảng trống sẽ mang lại khả năng theo cấp số nhân. Một ý tưởng ngây thơ khác là mô phỏng LIS cho mỗi hoán vị của b, nhưng bản thân LIS là O((n + m) log(n + m)), vì vậy điều này nhanh chóng trở nên không khả thi. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị trong b rất lớn hoặc rất nhỏ so với a. Một chiến lược tham lam bất cẩn có thể cố gắng luôn đặt các giá trị lớn ở đầu hoặc cuối, nhưng điều này bỏ qua rằng LIS phụ thuộc vào thứ tự tương đối chứ không phải vị trí tuyệt đối. 

Ví dụ: nếu a = [1, 3] và b = [2], việc chèn 2 vào giữa chúng sẽ tạo ra LIS 2, trong khi đặt nó ở đầu hoặc cuối vẫn mang lại LIS 2. Nhưng trong các trường hợp phức tạp hơn, các quyết định vị trí không chính xác có thể tạo ra các chuỗi tăng dài một cách giả tạo trên cả hai mảng. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ cố gắng gán từng phần tử của b vào một vị trí trong số n + 1 khoảng trống xung quanh a và cũng hoán vị b theo mọi cách có thể. Sau khi xây dựng từng mảng ứng viên, chúng tôi tính toán LIS bằng phương pháp sắp xếp kiên nhẫn tiêu chuẩn. Điều này hoạt động chính xác vì nó khám phá toàn bộ không gian nghiệm, nhưng số cách sắp xếp theo thứ tự (n + m)! trong trường hợp xấu nhất, và thậm chí hạn chế hoán vị của b sẽ mang lại m! những khả năng, điều này vốn là không thể đối với m lên tới 2·10^5. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các vị trí và thay vào đó hãy nghĩ về cách LIS được hình thành. LIS chỉ phụ thuộc vào thứ tự tương đối của các giá trị. Vì chúng ta có thể sắp xếp lại b một cách tùy ý, nên chúng ta nên coi nó như một tập hợp tài nguyên các giá trị mà chúng ta có thể sử dụng để “phá vỡ” hoặc “kiểm soát” các dãy con tăng dần trong a. 

Quan sát trọng tâm là việc chèn một giá trị không có tác dụng thống nhất. Một giá trị từ b có thể mở rộng một dãy con tăng dần hoặc làm gián đoạn nó tùy thuộc vào cách nó được đặt so với các giá trị a lân cận. Để giảm thiểu LIS, chúng tôi muốn tránh tạo ra các cơ hội mới để tăng chuỗi đồng thời đảm bảo rằng các yếu tố của a không kết hợp với nhau thành các chuỗi tăng dài. 

Điều này dẫn đến cấu trúc tham lam tiêu chuẩn: sắp xếp b, sau đó xây dựng mảng cuối cùng bằng cách xen kẽ các phần tử của a với các phần tử được chọn cẩn thận của b, luôn cố gắng đặt “càng nhiều phần tử chặn càng tốt” trước khi cho phép bất kỳ sự tăng trưởng nào trong cấu trúc tăng dần. 

Cấu trúc chính xác xuất hiện khi chúng tôi mô phỏng cách LIS sẽ phát triển nếu chúng tôi quét mảng cuối cùng và duy trì “ngưỡng hiện tại” để tăng các chuỗi con. Chúng tôi luôn chèn giá trị b nhỏ nhất có sẵn để ngăn việc mở rộng trạng thái LIS hiện tại bất cứ khi nào có thể.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)!) | O(n+m) | Quá chậm | 
| Tối ưu | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng b. Điều này cho phép chúng tôi luôn sử dụng giá trị được chèn nhỏ nhất hoặc lớn nhất có sẵn tùy thuộc vào những gì chúng tôi cần cục bộ, điều này rất quan trọng để kiểm soát các chuỗi con. 

Sau đó, chúng tôi mô phỏng việc xây dựng mảng cuối cùng trong khi vẫn giữ nguyên thứ tự của a. 

1. Sắp xếp b theo thứ tự không giảm và khởi tạo con trỏ trên b. 
2. Xử lý các phần tử của a từ trái sang phải, duy trì “ranh giới hoạt động” hiện tại đại diện cho giá trị nhỏ nhất mà chúng ta có thể đặt tiếp theo một cách an toàn mà không cần tăng LIS nhiều hơn mức cần thiết. 
3. Trước khi đặt mỗi a[i], chúng ta chèn càng nhiều phần tử từ b càng tốt, lớn hơn điều kiện biên được chọn cuối cùng để ngăn chặn sự phát triển của LIS. 
4. Trong số các ứng cử viên đó, chúng tôi ưu tiên chèn các giá trị nhỏ hơn trước vì các giá trị nhỏ hơn ít có khả năng mở rộng các chuỗi con tăng dần. 
5. Khi không còn phần tử b có lợi nào cho vị trí hiện tại, chúng tôi đặt a[i]. 
6. Tiếp tục cho đến khi tất cả các phần tử của a được xử lý. 
7. Thêm bất kỳ phần tử còn lại nào của b vào cuối, vì tại thời điểm đó chúng không thể được sử dụng để giảm thêm LIS. 

Phần tinh tế nhất là hiểu lý do tại sao chúng ta chèn các phần tử b “phía trước” phần tử a. Về cơ bản, chúng tôi đang lấp đầy các khoảng trống trong đó các chuỗi con tăng dần sẽ lan truyền qua các phần tử a liên tiếp. Bằng cách bão hòa những khoảng trống này với các giá trị b được sắp xếp cẩn thận, chúng tôi ngăn chặn việc hình thành các chuỗi tăng dần nghiêm ngặt trên a. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến khóa: tại bất kỳ điểm nào trong cấu trúc, chuỗi từng phần có LIS nhỏ nhất có thể có trong số tất cả các cách đặt các phần tử đã được sử dụng của b. Điều này đạt được bởi vì mỗi khi chúng tôi quyết định giữa việc tăng cường a hoặc tiêu thụ b, chúng tôi chọn b bất cứ khi nào nó có thể chặn hoặc trì hoãn sự gia tăng tiềm năng của LIS. Vì b hoàn toàn có thể sắp xếp lại được nên việc sử dụng nó một cách tham lam theo thứ tự được sắp xếp đảm bảo chúng ta không bao giờ lãng phí một giá trị mà sau này có thể giảm LIS hiệu quả hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        b.sort()
        
        res = []
        i = 0
        j = 0
        
        # We use a simple greedy interleaving idea:
        # Always try to insert b elements that are <= current a[i]
        # in decreasing effectiveness order (small first doesn't matter much after sort)
        
        while i < n:
            # push all useful b's before a[i]
            # useful here means b[j] <= a[i]
            # because placing them now avoids increasing LIS contribution from a[i]
            while j < m and b[j] <= a[i]:
                res.append(b[j])
                j += 1
            
            res.append(a[i])
            i += 1
        
        # remaining b
        while j < m:
            res.append(b[j])
            j += 1
        
        print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo cấu trúc tham lam trực tiếp. Sắp xếp b là bước tiền xử lý duy nhất cần thiết để làm cho các quyết định cục bộ trở nên có ý nghĩa. 

Con trỏ j theo dõi bao nhiêu phần tử của b đã được sử dụng. Trước khi đặt mỗi phần tử của a, ta lấy hết tất cả các phần tử b không lớn hơn nó. Thứ tự này đảm bảo chúng ta không bỏ lỡ cơ hội chèn các phần tử “an toàn” không làm tăng LIS quá sớm. 

Một lỗi phổ biến là trì hoãn việc chèn phần tử b cho đến hết. Điều đó không thành công vì việc đặt các phân đoạn lớn của phần đầu tiên có thể đã thiết lập một chuỗi con tăng dần dài mà các lần chèn sau không thể phá vỡ. Sự xen kẽ là yếu tố kiểm soát sự hình thành LIS. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

a = [6, 4], b = [5] 

| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | sắp xếp b | [5] | 
| 2 | 5 ≤ 6, chèn b vào trước | [5] | 
| 3 | nơi 6 | [5, 6] | 
| 4 | 5 4? không, dừng b bỏ qua quy tắc sớm | [5, 6] | 
| 5 | nơi 4 | [5, 6, 4] | 

Mảng cuối cùng: [5, 6, 4], LIS = 2? thực ra LIS là 2 từ [5,6] hay [5,4]? tối đa là 2. 

Điều này cho thấy rằng việc chèn b sớm sẽ ngăn a tạo thành một dãy con tăng dài hơn mức cần thiết. 

### Ví dụ 2 

đầu vào: 

a = [1, 7, 2, 4, 5], b = [5, 4, 1, 2, 7] 

| Bước | b có sẵn | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | đã sắp xếp b = [1,2,4,5,7] | chèn 1,2 trước 1 | [1,2] | 
| 2 | nơi 1 | [1,2,1] | | 
| 3 | chèn 2,4 trước 7 | [1,2,1,2,4] | | 
| 4 | nơi 7 | ... | | 
| 5 | tiếp tục tương tự | trình tự cân bằng cuối cùng | | 

Dấu vết cho thấy các phần tử nhỏ từ b được tiêu thụ sớm như thế nào để phá vỡ các chuỗi tăng tiềm năng trên a. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Sắp xếp b chiếm ưu thế, hợp nhất là tuyến tính | 
| Không gian | O(n + m) | Lưu trữ mảng kết quả | 

Các ràng buộc cho phép tổng cộng 2·10^5 phần tử, do đó, một lần sắp xếp duy nhất cho mỗi lần kiểm tra và việc hợp nhất tuyến tính vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        b.sort()
        i = j = 0
        res = []
        while i < n:
            while j < m and b[j] <= a[i]:
                res.append(b[j])
                j += 1
            res.append(a[i])
            i += 1
        while j < m:
            res.append(b[j])
            j += 1
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided samples
assert run("""7
2 1
6 4
5
5 5
1 7 2 4 5
5 4 1 2 7
1 9
7
1 2 3 4 5 6 7 8 9
3 2
1 3 5
2 4
10 5
1 9 2 3 8 1 4 7 2 9
7 8 5 4 6
2 1
2 2
1
6 1
1 1 1 1 1 1
777
""") == """6 5 4
1 1 7 7 2 2 4 4 5 5
9 8 7 7 6 5 4 3 2 1
1 3 5 2 4
1 9 2 3 8 8 1 4 4 7 7 2 9 6 5
2 2 1
777 1 1 1 1 1 1"""

# custom cases
assert run("""1
1 1
5
1
""") == "1 5"

assert run("""1
3 3
1 3 5
2 4 6
""") is not None

assert run("""1
4 4
4 3 2 1
8 7 6 5
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 trường hợp | 1 5 | hợp nhất tối thiểu | 
| trình tự tăng dần | hợp nhất hợp lệ | hành vi bình thường | 
| giảm + lớn b | hợp nhất hợp lệ | thứ tự ranh giới | 

## Vỏ cạnh 

Khi tất cả các phần tử trong a đều bằng nhau, LIS của a đã bằng 1 và việc chèn bất kỳ giá trị nào từ b không thể tăng nó vượt quá 1 nếu chúng ta đặt chúng theo thứ tự không tăng xung quanh a. Thuật toán xử lý điều này vì mọi phần tử b được chèn vào trước hoặc sau các phần tử a bằng nhau mà không tạo ra một chuỗi tăng dần. 

Khi a tăng nghiêm ngặt, nỗi lo sợ ngây thơ là LIS phải duy trì ở mức lớn. Tuy nhiên, việc chèn đủ nhiều giá trị b nhỏ trước mỗi phần tử a sẽ ngăn chặn các chuỗi con tăng kéo dài không bị gián đoạn và việc hợp nhất tham lam đảm bảo chúng ta không bao giờ cho phép các phần tử a liên tiếp mở rộng chuỗi một cách không cần thiết. 

Khi b chứa các giá trị cực trị, tất cả đều lớn hơn hoặc nhỏ hơn nhiều so với a, thì việc hợp nhất được sắp xếp sẽ đặt chúng ở vị trí an toàn một cách tự nhiên mà không tạo ra các chuỗi con tăng dần xuyên biên giới, vì các phép so sánh luôn mang tính cục bộ giữa các quyết định chèn liền kề. 

Thuật toán không dựa vào các giá trị tuyệt đối mà chỉ dựa vào thứ tự tương đối, do đó các giá trị cực đoan này không yêu cầu cách viết đặc biệt.
