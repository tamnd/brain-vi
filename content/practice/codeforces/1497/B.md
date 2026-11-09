---
title: "CF 1497B - M-mảng"
description: "Chúng ta có nhiều tập số và môđun $m$. Nhiệm vụ là sắp xếp lại tất cả các số thành càng ít chuỗi càng tốt, trong đó mỗi chuỗi phải đáp ứng quy tắc tương thích cục bộ: bất cứ khi nào hai phần tử liên tiếp xuất hiện trong cùng một chuỗi thì tổng của chúng phải chia hết…"
date: "2026-06-10T21:44:17+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1497
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 708 (Div. 2)"
rating: 1200
weight: 1497
solve_time_s: 142
verified: false
draft: false
---

[CF 1497B - M-array](https://codeforces.com/problemset/problem/1497/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều số và một mô-đun$m$. Nhiệm vụ là sắp xếp lại tất cả các số thành càng ít chuỗi càng tốt, trong đó mỗi chuỗi phải đáp ứng quy tắc tương thích cục bộ: bất cứ khi nào hai phần tử liên tiếp xuất hiện trong cùng một chuỗi, tổng của chúng phải chia hết cho$m$. 

Bởi vì chúng ta được phép hoán vị các phần tử một cách tùy ý bên trong mỗi chuỗi, hạn chế thực sự duy nhất là làm thế nào các phần tử “khớp” cạnh nhau dựa trên phần dư của chúng theo modulo$m$. Nếu hai số có số dư$r$Và$s$, thì chúng có thể liền kề nhau theo một trình tự nào đó một cách chính xác khi$r + s \equiv 0 \pmod m$. Điều này có nghĩa là mỗi phần còn lại$r$chỉ tương thích với phần bổ sung của nó$m - r$, trong khi phần còn lại$0$chỉ tương thích với chính nó và khi$m$chẵn, số dư$m/2$cũng có khả năng tự tương thích. 

Mục tiêu là chia tất cả các phần tử thành số lượng tối thiểu các chuỗi hợp lệ như vậy. 

Ràng buộc$n \le 10^5$trên tất cả các trường hợp thử nghiệm buộc chúng tôi phải tránh xa bất kỳ chiến lược nào thử tất cả các phân vùng hoặc xây dựng trình tự một cách rõ ràng. Bất kỳ giải pháp nào thậm chí ngầm xem xét tất cả các cặp hoặc thử quay lui sẽ thất bại, vì không gian tìm kiếm tăng theo cấp số nhân với$n$. Cấu trúc của bài toán gợi ý rằng chỉ tần số của số dư mới quan trọng chứ không phải sự sắp xếp thực sự của các phần tử. 

Một trường hợp thất bại tinh tế đối với lý luận ngây thơ là giả định rằng việc ghép các số dư bổ sung một cách tham lam luôn cực tiểu hóa các chuỗi. Ví dụ: nếu chúng ta cố gắng luôn khớp càng nhiều$r$với$m-r$nhất có thể và sau đó bắt đầu các trình tự mới cho phần còn sót lại mà không xem xét cấu trúc tổng thể, chúng ta có thể đánh giá quá cao số lượng trình tự cần thiết hoặc xây dựng các cấu trúc không hợp lệ. Một lỗi phổ biến khác là xử lý từng cặp số dư một cách độc lập mà không xử lý hành vi đặc biệt của phần dư.$0$Và$m/2$. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ cố gắng thực sự xây dựng từng trình tự một. Chúng ta có thể liên tục chọn một phần tử không được sử dụng, mở rộng một chuỗi bằng cách tham lam chọn bất kỳ phần tử chưa được sử dụng tương thích nào và tiếp tục cho đến khi không thể mở rộng được, sau đó bắt đầu một chuỗi mới. Về nguyên tắc, điều này đúng vì chúng ta rõ ràng tôn trọng quy tắc kề. Tuy nhiên, mỗi tiện ích mở rộng đều yêu cầu quét các phần tử tương thích không được sử dụng và trong trường hợp xấu nhất, chúng tôi có thể xem lại các phần tử nhiều lần. Điều này dẫn đến hành vi bậc hai, vượt xa giới hạn khi$n$đạt tới$10^5$. 

Quan sát quan trọng là tính kề chỉ phụ thuộc vào số dư và mỗi số dư$r$chỉ tương tác với một đối tác duy nhất$m-r$. Điều này làm giảm bài toán thành các bài toán đếm độc lập trên mỗi cặp dư lượng bổ sung. 

Đối với một cặp cố định$(r, m-r)$, chúng ta chỉ quan tâm đến việc tồn tại bao nhiêu phần tử của mỗi loại. Bất kỳ chuỗi hợp lệ nào chỉ sử dụng hai dư lượng này đều phải xen kẽ giữa chúng. Điều này biến vấn đề thành việc chia hai tập hợp thành số lượng chuỗi xen kẽ tối thiểu. Cấu trúc tối ưu xuất hiện khi chúng ta nhận ra rằng sự mất cân bằng giữa hai số đếm sẽ tạo ra nhiều chuỗi, vì một chuỗi xen kẽ duy nhất có thể hấp thụ tối đa sự chênh lệch một giữa các số đếm. 

Dư lượng đặc biệt$0$và khi nào$m$là chẵn,$m/2$, cư xử khác đi vì chúng ghép đôi với chính mình. Đối với những điều này, tất cả các phần tử có thể được đặt thành một chuỗi duy nhất vì mọi phần tử liền kề đều hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng trình tự Brute Force |$O(n^2)$|$O(n)$| Quá chậm | 
| Ghép nối phần còn lại dựa trên tần số |$O(n + m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén mảng thành số tần số của mỗi modulo còn lại$m$. 

1. Tính một mảng`cnt`Ở đâu`cnt[r]`lưu trữ bao nhiêu số còn lại$r$. Thao tác này sẽ xóa tất cả thông tin đặt hàng và chỉ giữ lại những thông tin quan trọng đối với tính khả thi của vùng lân cận. 
2. Xử lý phần còn lại$0$. Nếu như`cnt[0]`khác 0, tất cả các phần tử như vậy có thể được đặt vào một chuỗi hợp lệ duy nhất, vì$0 + 0 \equiv 0 \pmod m$. Điều này đóng góp 0 hoặc 1 chuỗi. 
3. Nếu$m$chẵn, xử lý phần còn lại$m/2$. Tất cả các phần tử có phần dư này cũng tương thích lẫn nhau, vì vậy chúng có thể được đặt thành một chuỗi duy nhất nếu chúng tồn tại. Điều này cũng đóng góp nhiều nhất là 1 chuỗi. 
4. Đối với mỗi phần còn lại$r$từ$1$ĐẾN$m-1$, ghép nối nó với$m-r$, nhưng chỉ xử lý mỗi cặp một lần bằng cách thực thi$r < m-r$. Đối với mỗi cặp như vậy, chúng tôi quyết định cần bao nhiêu chuỗi để chứa cả hai nhóm. 
5. Cho một cặp$(r, m-r)$, cho phép$a = cnt[r]$Và$b = cnt[m-r]$. Nếu cả hai đều bằng 0, chúng ta bỏ qua nó. Mặt khác, số lượng chuỗi xen kẽ tối thiểu cần thiết là$\max(1, |a - b|)$. Điều này cho thấy thực tế là một chuỗi xen kẽ duy nhất có thể hấp thụ càng nhiều sự cân bằng càng tốt và bất kỳ sự mất cân bằng nào còn sót lại sẽ tạo ra các chuỗi bổ sung. 
6. Tổng hợp ý kiến ​​đóng góp của các nhóm độc lập để có được đáp án cuối cùng. 

### Tại sao nó hoạt động 

Mỗi chuỗi trên một cặp$(r, m-r)$là một chuỗi xen kẽ và trong bất kỳ chuỗi đơn nào, sự khác biệt giữa số lượng của hai dư lượng nhiều nhất là một. Do đó, một trình tự có thể “hấp thụ” các cặp cân bằng một cách hiệu quả và chỉ phần thừa dư thừa sẽ tạo ra các trình tự mới. Bởi vì các cặp dư lượng khác nhau không tương tác, nên việc tối ưu hóa từng cặp độc lập sẽ tạo ra sự phân hủy tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        cnt = [0] * m
        for x in a:
            cnt[x % m] += 1
        
        ans = 0
        
        if cnt[0] > 0:
            ans += 1
        
        if m % 2 == 0 and cnt[m // 2] > 0:
            ans += 1
        
        for r in range(1, m):
            s = m - r
            if r >= s:
                continue
            a1, a2 = cnt[r], cnt[s]
            if a1 == 0 and a2 == 0:
                continue
            ans += max(1, abs(a1 - a2))
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng tần số còn lại, đây là thông tin duy nhất phù hợp với tính khả thi. Sau đó, nó xử lý riêng biệt hai lớp dư lượng tự tương thích, vì chúng hoạt động giống như các vấn đề của một nhóm độc lập. 

Vòng lặp kết thúc$r$ghép mỗi dư lượng với phần bù của nó đúng một lần. biểu thức`max(1, abs(a1 - a2))`triển khai kết quả cấu trúc quan trọng cho các chuỗi xen kẽ: ngay cả một cặp mất cân bằng nặng vẫn cần ít nhất một chuỗi và mỗi đơn vị mất cân bằng bổ sung sẽ tạo ra một chuỗi bổ sung. 

Phải cẩn thận để bỏ qua việc đếm hai lần bằng cách đảm bảo$r < m-r$, nếu không thì mỗi cặp sẽ được xử lý hai lần và câu trả lời sẽ bị tăng cao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=6, m=4
a = [2, 2, 8, 6, 9, 4]
```Số dư là:```
0: 1 element (4)
1: 1 element (9)
2: 3 elements (2,2,8,6 → actually 4 elements mod 4 gives 0,2,2,2 depending; final counts matter)
3: 0 elements
```| Bước | Cặp | Đếm | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | r=0 | cnt[0]=1 | +1 | 
| 2 | r=1,3 | 1 vs 0 | tối đa(1,1)=1 | 
| 3 | r=2 (tự khi m chẵn? m/2=2) | cnt[2]>0 | +1 | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy mỗi thành phần dư lượng độc lập đóng góp riêng biệt và kết hợp bổ sung như thế nào. 

### Ví dụ 2 

đầu vào:```
n=5, m=3
a = [1, 2, 2, 1, 1]
```Phần còn lại:```
0: 0
1: 3
2: 2
```| Bước | Cặp | Đếm | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | r=1,2 | 3 vs 2 | tối đa(1,1)=1 | 

Câu trả lời cuối cùng là 1. 

Điều này chứng tỏ một cấu trúc xen kẽ được kết nối đầy đủ trong đó sự mất cân bằng đủ nhỏ để được hấp thụ thành một chuỗi duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$mỗi bài kiểm tra | Việc đếm phần dư mất thời gian tuyến tính ở kích thước đầu vào và phần dư ghép nối là tuyến tính ở$m$| 
| Không gian |$O(m)$| Mảng tần số cho cặn | 

Tổng các ràng buộc đảm bảo$\sum n + \sum m \le 10^5$, vì vậy cách tiếp cận tuyến tính này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            cnt = [0] * m
            for x in a:
                cnt[x % m] += 1

            ans = 0
            if cnt[0] > 0:
                ans += 1
            if m % 2 == 0 and cnt[m // 2] > 0:
                ans += 1

            for r in range(1, m):
                s = m - r
                if r >= s:
                    continue
                a1, a2 = cnt[r], cnt[s]
                if a1 == 0 and a2 == 0:
                    continue
                ans += max(1, abs(a1 - a2))

            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
6 4
2 2 8 6 9 4
10 8
1 1 1 5 2 4 4 8 6 7
1 1
666
2 2
2 4
""") == """3
6
1
1"""

# all equal values
assert run("""1
5 3
3 3 3 3 3
""") == "1"

# single element
assert run("""1
1 7
10
""") == "1"

# no pairing possible
assert run("""1
4 5
1 1 1 1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mẫu | 3 6 1 1 | đúng đắn về cấu trúc hỗn hợp | 
| tất cả các giá trị bằng nhau | 1 | tự bổ sung sụp đổ | 
| phần tử đơn | 1 | chuỗi tối thiểu không trống | 
| cặn đồng nhất | 4 | chia tách tối đa do mất cân bằng | 

## Vỏ cạnh 

Đối với phần còn lại$0$, thuật toán sẽ gán chính xác một chuỗi đơn bất cứ khi nào nó tồn tại, bởi vì tất cả các phần tử như vậy đều tương thích lẫn nhau. Ngay cả khi có nhiều số 0, chúng cũng không ép buộc nhiều chuỗi vì không có ràng buộc xen kẽ nào bị vi phạm. 

Đối với phần còn lại$m/2$khi$m$là số chẵn, lý luận tương tự cũng được áp dụng. Mặc dù nó có thể trông giống như một vấn đề ghép đôi, nhưng mọi cặp dư lượng giống hệt nhau đều hợp lệ, do đó tất cả các phần tử có thể được xâu chuỗi thành một chuỗi bất kể số lượng. 

Đối với các cặp bổ sung mất cân bằng cao, biểu thức$\max(1, |a-b|)$đảm bảo rằng ngay cả khi một phần dư chiếm ưu thế, thì ít nhất một chuỗi được hình thành để trùng lặp có thể sử dụng được, trong khi phần dư thừa còn lại được buộc chính xác thành các chuỗi đơn lẻ hoặc được lấp đầy một phần riêng biệt.
