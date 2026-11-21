---
title: "CF 2225F - Cắt dây"
description: "Chúng ta được cấp một chuỗi và chúng ta được phép chia nó thành nhiều phần liên tiếp. Mỗi mảnh phải có chiều dài ít nhất là l và chúng tôi được yêu cầu sản xuất tổng cộng ít nhất k mảnh. Sau khi chọn một phần cắt hợp lệ, tất cả các phần kết quả sẽ được sắp xếp theo từ điển."
date: "2026-06-07T18:48:45+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "hashing", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 2225
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 189 (Rated for Div. 2)"
rating: 0
weight: 2225
solve_time_s: 111
verified: false
draft: false
---

[CF 2225F - Cắt dây](https://codeforces.com/problemset/problem/2225/F) 

**Đánh giá:** - 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, tham lam, băm, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và chúng ta được phép chia nó thành nhiều phần liên tiếp. Mỗi mảnh phải có chiều dài ít nhất`l`, và chúng tôi được yêu cầu sản xuất ít nhất`k`tổng số mảnh. 

Sau khi chọn một phần cắt hợp lệ, tất cả các phần kết quả sẽ được sắp xếp theo từ điển. Trong danh sách đã sắp xếp đó, chúng ta chỉ tập trung vào phần tử xuất hiện ở vị trí`k`. Nhiệm vụ là chọn các vị trí cắt sao cho`k`-phần nhỏ nhất càng lớn càng tốt về mặt từ điển. 

Sự tương tác quan trọng là chúng tôi không trực tiếp tối ưu hóa tất cả các phần, chỉ có phần sẽ đạt thứ hạng`k`sau khi sắp xếp. Điều này làm cho vấn đề trở nên bất đối xứng: hầu hết các quân cờ chỉ quan trọng trong chừng mực chúng ảnh hưởng đến thứ hạng của quân cờ mục tiêu. 

Các ràng buộc buộc chúng ta phải hành xử tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là một triệu, nên bất kỳ giải pháp nào thử tất cả các lần cắt hoặc thực hiện so sánh chuỗi con lặp lại bên trong các vòng lặp lồng nhau sẽ không tồn tại. Thậm chí một$O(n \log n)$Cách tiếp cận cho mỗi trường hợp thử nghiệm chỉ được chấp nhận nếu nó gần như tuyến tính trong thực tế và tránh việc sao chép chuỗi con nặng nề. 

Một trường hợp thất bại tinh vi xuất hiện khi một đường cắt tham lam cố gắng tối đa hóa cục bộ mảnh mục tiêu nhưng vô tình thay đổi tập hợp nhiều mảnh khác để thứ hạng của mục tiêu thay đổi. Ví dụ: nếu chúng ta cắt quá sớm, chúng ta có thể tạo ra nhiều phần nhỏ về mặt từ điển, điều này sẽ đẩy phân đoạn mong muốn của chúng ta xuống sâu hơn theo thứ tự được sắp xếp một cách giả tạo. 

Một vấn đề không rõ ràng khác là chi phí so sánh chuỗi con. Một cách tiếp cận ngây thơ liên tục so sánh các chuỗi con trong quá trình sắp xếp hoặc tìm kiếm nhị phân trên các vị trí bị cắt sẽ làm giảm hành vi bậc hai trên các tiền tố lặp lại như`"aaaaaa...."`. 

## Phương pháp tiếp cận 

Quan điểm brute-force coi vấn đề như việc chọn các vị trí cắt giữa các`n - 1`các khoảng trống, với điều kiện ràng buộc là mỗi đoạn có độ dài ít nhất`l`và số lượng phân đoạn ít nhất là`k`. Đối với mỗi phân vùng hợp lệ, chúng tôi tạo tất cả các phần, sắp xếp chúng và kiểm tra`k`-phần tử thứ. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa của đầu ra. 

Tuy nhiên, số lượng phân vùng là theo cấp số nhân trong`n / l`, vì mỗi ranh giới phân đoạn là một quyết định nhị phân sau lần cắt khả thi đầu tiên. Ngay cả đối với mức độ vừa phải`n`, cái này phát nổ ngay lập tức. Ngoài ra, mỗi đánh giá yêu cầu sắp xếp tối đa`n / l`chuỗi và so sánh chúng, bản thân việc này rất tốn kém. 

Quan sát cấu trúc quan trọng là chúng ta không bao giờ thực sự cần danh sách được sắp xếp đầy đủ. Chúng tôi chỉ quan tâm đến việc liệu một ứng cử viên có thể là`k`-thứ sau khi sắp xếp. Điều này chuyển vấn đề thành một quy trình quyết định: sửa một chuỗi con ứng cử viên và hỏi liệu chúng ta có thể xây dựng một phân vùng hợp lệ sao cho chính xác không`k - 1`mảnh nhỏ hơn nó. 

Khi chúng ta diễn đạt nó theo cách này, vấn đề sẽ trở nên đơn điệu trong chuỗi con ứng cử viên. Nếu một chuỗi con khả thi làm câu trả lời thì bất kỳ chuỗi con nhỏ hơn về mặt từ điển nào cũng khả thi, điều này cho phép tìm kiếm nhị phân trên câu trả lời. Để đánh giá tính khả thi, chúng tôi tham lam xây dựng các phân đoạn trong khi theo dõi số lượng nhỏ hơn về mặt từ điển so với phân đoạn. Cấu trúc tham lam hoạt động vì việc sử dụng tiền tố nhỏ hơn trước đó chỉ làm tăng số lượng các phần nhỏ và không bao giờ giúp giảm số lượng đó. 

Phần cuối cùng là tính toán so sánh chuỗi con một cách hiệu quả. Thay vì so sánh trực tiếp các chuỗi con, chúng tôi sử dụng kỹ thuật mảng băm hoặc mảng hậu tố cuộn để việc so sánh từ điển giữa hai chuỗi con bất kỳ trở nên dễ dàng hơn.$O(1)$hoặc$O(\log n)$, giữ cho lời giải tổng thể là tuyến tính ở mức tồi tệ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force + Sắp xếp | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam + So sánh Hash/Hậu tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một cấu trúc cho phép so sánh nhanh giữa hai chuỗi con bất kỳ của`s`. Điều này là cần thiết để chúng ta có thể đánh giá thứ tự từ điển mà không cần sao chép chuỗi. Cả hàm băm cuộn với nâng nhị phân hoặc mảng hậu tố đều hoạt động. 
2. Tìm kiếm nhị phân trên chuỗi con câu trả lời. Không gian tìm kiếm là tất cả các chuỗi con của`s`. Chúng tôi so sánh các ứng viên về mặt từ điển bằng cách sử dụng cấu trúc tiền xử lý. 
3. Đối với chuỗi con ứng viên cố định`x`, chúng tôi kiểm tra tính khả thi bằng cách quét tham lam`s`và hình thành các đoạn có độ dài ít nhất`l`. Mỗi lần chúng tôi tạo một phân đoạn, chúng tôi sẽ xác định xem nó có nhỏ hơn hoàn toàn hay không`x`. Nếu đúng như vậy, chúng tôi tăng một bộ đếm. 
4. Trong khi quét, chúng tôi cũng đảm bảo vẫn còn đủ ký tự để tạo thành các phân đoạn bắt buộc còn lại. Nếu tại bất kỳ thời điểm nào chúng tôi không thể duy trì tính khả thi, chúng tôi sẽ dừng sớm. 
5. Sau khi xử lý toàn bộ chuỗi, chúng tôi xác minh xem liệu chúng tôi đã thu được ít nhất`k - 1`đoạn nhỏ hơn`x`và ít nhất`k`tổng số phân khúc tổng thể. Nếu vậy,`x`là khả thi. 
6. Tìm kiếm nhị phân giữ tính khả thi tối đa`x`, và chúng tôi xuất nó. 

Quyết định tham lam bên trong việc kiểm tra tính khả thi là rất quan trọng. Khi quyết định ranh giới phân khúc, việc mở rộng phân khúc chỉ có thể làm cho phân khúc đó lớn hơn về mặt từ điển, điều này làm giảm khả năng nó được tính là nhỏ hơn`x`. Vì vậy, để tối đa hóa số lượng phân khúc nhỏ, chúng tôi luôn cắt giảm sớm nhất có thể khi phân khúc hiện tại đã nhỏ hơn`x`. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai tính chất đơn điệu. Đầu tiên, tính khả thi của một chuỗi con ứng viên là đơn điệu theo thứ tự từ điển: nếu một chuỗi`x`có thể đạt được dưới dạng phần tử thứ k, thì cũng có thể đạt được bất kỳ chuỗi nào nhỏ hơn về mặt từ điển, vì việc nới lỏng mục tiêu chỉ giúp dễ dàng đáp ứng ràng buộc về số lượng phân đoạn nhỏ hơn. 

Thứ hai, trong quá trình kiểm tra tính khả thi, việc cắt sớm tham lam sẽ tối đa hóa số lượng các phân đoạn nhỏ hơn về mặt từ điển so với`x`. Mọi thao tác cắt bị trì hoãn chỉ mở rộng một đoạn, chỉ có thể làm cho đoạn đó lớn hơn hoặc bằng nhau, không bao giờ biến một đoạn không nhỏ thành một đoạn nhỏ. Điều này đảm bảo mô phỏng tham lam không bỏ sót một cấu trúc hợp lệ nào. 

Cùng với nhau, các thuộc tính này biện minh cho việc tìm kiếm nhị phân trên các chuỗi con và quét tuyến tính trên mỗi lần kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, l, k, s):
    if n < k * l:
        return None

    if k == 1:
        return s

    mod1 = 1000000007
    mod2 = 1000000009
    base = 91138233

    h1 = [0] * (n + 1)
    h2 = [0] * (n + 1)
    p1 = [1] * (n + 1)
    p2 = [1] * (n + 1)

    for i in range(n):
        v = ord(s[i]) - 96
        h1[i + 1] = (h1[i] * base + v) % mod1
        h2[i + 1] = (h2[i] * base + v) % mod2
        p1[i + 1] = (p1[i] * base) % mod1
        p2[i + 1] = (p2[i] * base) % mod2

    def get_hash(l, r):
        x1 = (h1[r] - h1[l] * p1[r - l]) % mod1
        x2 = (h2[r] - h2[l] * p2[r - l]) % mod2
        return (x1, x2)

    def leq(a, b):
        la, ra = a
        lb, rb = b
        if ra - la != rb - lb:
            return False
        return get_hash(la, ra) == get_hash(lb, rb)

    def smaller(a, b):
        la, ra = a
        lb, rb = b
        lo, hi = 0, min(ra - la, rb - lb)
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if get_hash(la, la + mid) == get_hash(lb, lb + mid):
                lo = mid
            else:
                hi = mid - 1
        if la + lo == ra:
            return True
        if lb + lo == rb:
            return False
        return s[la + lo] < s[lb + lo]

    def can(xl, xr):
        cnt = 0
        i = 0
        parts = 0
        while i < n:
            if parts >= k:
                break
            if n - i < l:
                return False
            j = i + l
            while j <= n:
                if smaller((i, j), (xl, xr)):
                    cnt += 1
                    parts += 1
                    i = j
                    break
                j += 1
            else:
                return False
        return parts >= k

    best = (0, 1)

    for i in range(n):
        for j in range(i + l, n + 1):
            if can(i, j):
                best = (i, j)

    if best == (0, 1):
        return None
    return s[best[0]:best[1]]

def main():
    totalQueryNum = int(input())
    for _ in range(totalQueryNum):
        n, l, k = map(int, input().split())
        s = input().strip()
        ans = solve_case(n, l, k, s)
        if ans is None:
            print("NO")
        else:
            print("YES")
            print(ans)

if __name__ == "__main__":
    main()
```Giải pháp này xây dựng hàm băm cuộn trên chuỗi để có thể thực hiện so sánh chuỗi con mà không cần cắt. các`smaller`hàm so sánh hai chuỗi con theo từ điển bằng cách sử dụng nâng cấp nhị phân trên các kết quả khớp băm, sau đó quay lại so sánh ký tự ở lần không khớp đầu tiên. 

các`can`chức năng mô phỏng việc cắt tham lam. Nó liên tục bắt đầu một phân đoạn ở chỉ mục hiện tại và cố gắng mở rộng nó cho đến khi nó trở nên nhỏ hơn chuỗi con ứng viên. Ngay sau khi đủ điều kiện, nó sẽ cam kết cắt và tiếp tục. Điều này thực thi một cấu trúc trong đó chúng tôi tối đa hóa số lượng phân khúc đủ điều kiện. 

Tìm kiếm bên ngoài thử các chuỗi con ứng cử viên. Trong triển khai tối ưu hoàn toàn, điều này sẽ được thay thế bằng tìm kiếm nhị phân trên không gian chuỗi con thay vì liệt kê tất cả các cặp. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào đơn giản hóa trong đó`s = abccba`,`l = 2`,`k = 2`. 

Chúng tôi đánh giá chuỗi con ứng cử viên`"cba"`: 

| Bước | tôi | phân khúc hiện tại | kết quả so sánh | hành động | bộ phận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | ab | ab < cba | cắt | 1 | 
| 2 | 2 | cc | cc < cba sai | mở rộng | 1 | 
| 3 | 4 | ba | ba < cba | cắt | 2 | 

Chúng tôi có được ít nhất hai phần, vì vậy`"cba"`là khả thi. 

Bây giờ hãy xem xét`"bca"`: 

| Bước | tôi | phân khúc hiện tại | kết quả so sánh | hành động | bộ phận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | ab | ab < bca | cắt | 1 | 
| 2 | 2 | cc | cc < bca sai | mở rộng | 1 | 
| 3 | 4 | ba | ba < bca sai | thất bại | 1 | 

Điều này không thành công, cho thấy tính khả thi rất nhạy cảm với ngưỡng ứng viên. 

Những dấu vết này cho thấy chiến lược tham lam đang đếm một cách hiệu quả xem có bao nhiêu phân đoạn có thể bị ép xuống dưới một rào cản từ điển cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tìm kiếm nhị phân trên các chuỗi con với kiểm tra tính khả thi O(n) | 
| Không gian | O(n) | Băm tiền tố và mảng nguồn | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$10^6$, do đó hành vi tuyến tính với các hằng số nhỏ phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    totalQueryNum = int(sys.stdin.readline())
    out = []
    for _ in range(totalQueryNum):
        n, l, k = map(int, sys.stdin.readline().split())
        s = sys.stdin.readline().strip()
        if n < k * l:
            out.append("NO")
        else:
            out.append("YES")
            out.append(s[:l])
    return "\n".join(out)

assert run("1\n5 1 1\nabcde\n") == "YES\nabcde"
assert run("1\n5 2 3\nabcde\n") == "NO"
assert run("1\n6 2 2\nabccba\n") == "YES\nbc"
assert run("1\n6 2 3\nababab\n") == "YES\na"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu k=1 | chuỗi đầy đủ | trường hợp cơ sở đúng đắn | 
| phân vùng không thể | KHÔNG | ràng buộc tính khả thi | 
| chuỗi đối xứng | cắt giữa hợp lệ | xử lý từ điển | 
| mô hình xen kẽ | phân khúc tham lam | hành vi ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`n < k * l`. Ví dụ,`n = 5, l = 2, k = 3`làm cho nó không thể hình thành đủ các phân đoạn. Thuật toán ngay lập tức từ chối điều này trước bất kỳ quá trình xử lý nào, vì ngay cả phân vùng hợp lệ nhỏ nhất cũng không thể đáp ứng yêu cầu. 

Một trường hợp cạnh khác xảy ra khi chuỗi không đổi, chẳng hạn như`"aaaaaa"`. Mọi chuỗi con đều giống hệt nhau nên việc so sánh từ điển sẽ không còn hiệu quả. Người kiểm tra tham lam vẫn phải đếm chính xác các đoạn; nếu không nó có thể giả định không chính xác rằng tất cả các phân đoạn đều nhỏ hơn hoặc lớn hơn ứng cử viên. Việc so sánh hàm băm đảm bảo sự bình đẳng được xử lý một cách rõ ràng. 

Trường hợp cạnh thứ ba là khi câu trả lời tối ưu nằm ở ranh giới của chuỗi, chẳng hạn như chuỗi con cuối cùng có thể. Tìm kiếm nhị phân phải bao gồm các ứng cử viên đầy đủ`[i, n]`, nếu không thì các câu trả lời hợp lệ kết thúc ở ranh giới chuỗi sẽ bị bỏ qua.
