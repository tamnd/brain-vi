---
title: "CF 104460E - Tắt nó đi"
description: "Chúng ta được cung cấp một mảng nhị phân biểu thị một dãy đèn, trong đó mỗi vị trí bật hoặc tắt. Chúng ta được phép thực hiện thao tác chọn chỉ mục bắt đầu và tắt một đoạn liền kề có độ dài cố định $L$."
date: "2026-06-30T13:29:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104460
codeforces_index: "E"
codeforces_contest_name: "The 2019 ICPC China Shaanxi Provincial Programming Contest"
rating: 0
weight: 104460
solve_time_s: 47
verified: true
draft: false
---

[CF 104460E - Tắt nó đi](https://codeforces.com/problemset/problem/104460/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân biểu thị một dãy đèn, trong đó mỗi vị trí bật hoặc tắt. Chúng ta được phép thực hiện thao tác chọn chỉ số bắt đầu và tắt một đoạn liền kề có độ dài cố định$L$. Mọi hoạt động luôn sử dụng cùng một$L$, và chúng tôi có thể áp dụng nhiều nhất$k$những hoạt động như vậy. Mục tiêu là xác định độ dài đoạn nhỏ nhất có thể$L$sao cho tất cả các đèn bật ban đầu có thể được tắt trong phạm vi số lần thao tác cho phép. 

Khía cạnh quan trọng là mỗi thao tác không thích ứng về độ dài mà chỉ thích ứng về vị trí. Một lần$L$đã được sửa, quyền tự do duy nhất là đặt cửa sổ ở đâu và chúng tôi muốn biết liệu tất cả những cái trong chuỗi có thể được bao phủ bởi nhiều nhất hay không$k$các khoảng có độ dài đó. 

Các ràng buộc đủ lớn để bất kỳ mô phỏng bậc hai nào trên tất cả các lựa chọn của$L$và mọi vị trí đều không thể thực hiện được. Từ$n$có thể đạt được$2 \times 10^5$cho mỗi trường hợp thử nghiệm và tổng số tiền đạt$2 \times 10^6$, chúng ta phải nhắm mục tiêu gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận cố gắng mô phỏng các hoạt động trên mỗi ứng viên$L$một cách ngây thơ. 

Một điểm tinh tế là sự chồng chéo không thành vấn đề ngoại trừ phạm vi bao phủ. Chúng ta không bao giờ cần lập mô hình trạng thái bật tắt chính xác trong quá trình vận hành; chỉ liệu tất cả các vị trí chứa '1' có được bao phủ bởi nhiều nhất hay không$k$khoảng thời gian. 

Một trường hợp lỗi phổ biến xuất hiện khi quá trình triển khai thử vị trí tham lam mà không theo dõi chính xác khoảng cách mà hoạt động trước đó đã thực hiện. 

Ví dụ, hãy xem xét:```
n = 5, k = 1
s = 10001
```Nếu như$L = 3$, một người tham lam ngây thơ có thể đặt khoảng bắt đầu từ 1 để bao phủ khoảng 1 đầu tiên, nhưng sẽ bỏ sót khoảng thời gian cuối cùng. Vị trí chính xác sẽ không thành công bất kể vì một khoảng không thể bao gồm hai vùng riêng biệt vượt quá độ dài 3. Bất kỳ cách tiếp cận nào không giải thích chính xác về giới hạn phạm vi sẽ đánh giá sai tính khả thi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với một cố định$L$, chúng ta quét chuỗi từ trái sang phải. Bất cứ khi nào chúng tôi gặp số '1' chưa được che phủ, chúng tôi sẽ thực hiện một thao tác bắt đầu từ vị trí đó và che phủ vị trí tiếp theo.$L$tế bào. Chúng tôi lặp lại cho đến khi tất cả những cái đó được bao phủ hoặc chúng tôi vượt quá$k$hoạt động. Điều này đúng vì đặt khoảng thời gian càng sớm càng tốt không bao giờ làm giảm tính linh hoạt trong tương lai. 

Vấn đề là việc kiểm tra một$L$mất$O(n)$, và thử tất cả$L$giá trị từ 1 đến$n$dẫn đến$O(n^2)$, quá lớn đối với$n = 2 \times 10^5$. 

Quan sát quan trọng là tính khả thi là đơn điệu trong$L$. Nếu nhất định$L$là đủ để bao gồm tất cả những người trong nhiều nhất$k$hoạt động, sau đó lớn hơn$L$cũng đủ vì mỗi thao tác bao gồm nhiều ô hơn và không bao giờ tăng số lượng thao tác cần thiết. Điều này cho phép chúng tôi tìm kiếm nhị phân câu trả lời. 

Bây giờ vấn đề giảm xuống còn việc kiểm tra tính khả thi của một giải pháp cố định$L$, có thể được thực hiện một cách tham lam trong thời gian tuyến tính. Chúng tôi quét chuỗi và bất cứ khi nào chúng tôi tìm thấy số '1' không được phát hiện, chúng tôi đặt một khoảng bắt đầu tại vị trí đó và chuyển tiếp theo$L$. Số lượng khoảng thời gian được sử dụng được so sánh với$k$. 

Sự kết hợp giữa kiểm tra tính khả thi và tìm kiếm nhị phân này mang lại một giải pháp hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với L bằng cách kiểm tra tham lam |$O(n^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tham lam |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng tìm kiếm nhị phân trên$L$. 

1. Xác định hàm`can(L)`kiểm tra xem liệu tất cả các vị trí '1' có thể được bao phủ bằng cách sử dụng tối đa$k$các đoạn có độ dài$L$. Chúng tôi mô phỏng từ trái sang phải, theo dõi đoạn được đặt cuối cùng đạt được bao xa. 
2. Bên trong`can(L)`, chúng tôi duy trì một con trỏ`i`quét chuỗi và bộ đếm`cnt`cho biết chúng tôi đã sử dụng bao nhiêu phân đoạn. Khi chúng ta gặp phải một vị trí`i`với số '1' chưa được đề cập, chúng tôi tăng dần`cnt`và đặt phạm vi bảo hiểm mở rộng từ`i`ĐẾN`i + L - 1`. 
3. Sau đó, chúng tôi bỏ qua tất cả các vị trí thuộc phân khúc này bằng cách di chuyển`i`phía trước. Điều này đảm bảo mỗi phân đoạn được đặt một cách tham lam ở vị trí '1' chưa được khám phá sớm nhất có thể, giảm thiểu phạm vi phủ sóng lãng phí. 
4. Nếu tại bất kỳ thời điểm nào`cnt > k`, chúng ta dừng sớm và trả về false vì đã vượt quá số thao tác cho phép. 
5. Thực hiện tìm kiếm nhị phân trên$L$từ 1 đến$n$. Đối với mỗi điểm giữa, hãy gọi`can(mid)`. Nếu khả thi, chúng tôi thử các giá trị nhỏ hơn; nếu không, chúng tôi tăng$L$. 
6. Xuất ra nhỏ nhất$L$vì cái gì`can(L)`trả về đúng. 

Vị trí tham lam là tối ưu cho cố định$L$bởi vì việc đặt một phân đoạn muộn hơn phân đoạn '1' chưa được khám phá đầu tiên không thể làm giảm số lượng các phân đoạn cần thiết và chỉ có thể để lại những phân đoạn bổ sung chưa được khám phá. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào độ bao phủ bất biến. Sau mỗi phân đoạn được đặt, tất cả các vị trí cho đến điểm cuối bên phải của nó đều được giải quyết hoàn toàn, nghĩa là không có quyết định nào trong tương lai phụ thuộc vào chúng. Mỗi số '1' không được che chắn sẽ buộc ít nhất một phân đoạn phải bắt đầu tại hoặc trước vị trí đó. Chiến lược tham lam luôn đáp ứng yêu cầu này trong thời gian sớm nhất có thể, đảm bảo không có phân khúc nào bị lãng phí trên các khu vực đã được bao phủ. Do đó, số lượng phân đoạn được tạo ra là tối thiểu cho điều đó$L$, làm cho việc kiểm tra tính khả thi trở nên chính xác. 

Bởi vì tính khả thi là đơn điệu trong$L$, tìm kiếm nhị phân trên$L$là hợp lệ và sự kết hợp mang lại mức tối thiểu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(s, n, k, L):
    cnt = 0
    i = 0
    while i < n:
        if s[i] == '1':
            cnt += 1
            if cnt > k:
                return False
            i += L
        else:
            i += 1
    return True

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    lo, hi = 1, n
    ans = n

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(s, n, k, mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```chức năng`can`là sự thực hiện trực tiếp của quá trình bao phủ tham lam. Chi tiết quan trọng là khi chúng ta nhấn số '1', chúng ta ngay lập tức thực hiện một thao tác và nhảy về phía trước bằng cách$L$, bởi vì bất kỳ giải pháp tối ưu nào cũng phải bao trùm vị trí đó trong một khoảng thời gian nào đó và việc trì hoãn chỉ gây rủi ro cho các hoạt động bổ sung sau này. 

Tìm kiếm nhị phân được thực hiện theo cách chuẩn, thu hẹp vùng khả thi cho đến khi giá trị hợp lệ nhỏ nhất$L$được tìm thấy. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 5, k = 2
s = 10101
```Chúng tôi đánh giá tính khả thi của$L = 2$. 

| tôi | s[i] | hành động | cnt | tiếp theo tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | khoảng cách vị trí [0,1] | 1 | 2 | 
| 2 | 1 | khoảng cách vị trí [2,3] | 2 | 4 | 
| 4 | 1 | khoảng cách vị trí [4,5] | 3 | dừng lại | 

Chúng tôi đã sử dụng 3 thao tác, vượt quá$k=2$, Vì thế$L=2$là không thể thực hiện được. 

Bây giờ hãy xem xét$L = 3$. 

| tôi | s[i] | hành động | cnt | tiếp theo tôi | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | nơi [0,2] | 1 | 3 | 
| 3 | 0 | bỏ qua | 1 | 4 | 
| 4 | 1 | nơi [4,6] | 2 | 7 | 

Ở đây chúng ta sử dụng đúng 2 thao tác, vì vậy$L=3$là khả thi. 

Điều này cho thấy ngày càng tăng$L$giảm số lượng các phân đoạn cần thiết và thể hiện tính đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi kiểm tra tính khả thi là tuyến tính và tìm kiếm nhị phân chạy qua$O(\log n)$giá trị | 
| Không gian |$O(1)$| Chỉ sử dụng một số bộ đếm và con trỏ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$2 \times 10^6$và hệ số logarit vẫn nhỏ, làm cho giải pháp đủ hiệu quả cho các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def can(s, n, k, L):
        cnt = 0
        i = 0
        while i < n:
            if s[i] == '1':
                cnt += 1
                if cnt > k:
                    return False
                i += L
            else:
                i += 1
        return True

    def solve():
        n, k = map(int, input().split())
        s = input().strip()

        lo, hi = 1, n
        ans = n

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(s, n, k, mid):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        return ans

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# minimum size
assert run("1\n1 1\n1\n") == "1"

# already all zeros except one far
assert run("1\n5 1\n00001\n") == "1"

# all ones
assert run("1\n5 1\n11111\n") == "5"

# multiple segments needed
assert run("1\n6 2\n101010\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 1 | hành vi phân khúc tối thiểu | 
| thưa xa 1 | 1 | nhảy đúng | 
| tất cả những cái, k=1 | n | khoảng đơn trong trường hợp xấu nhất | 
| mô hình xen kẽ | 3 | nhiều vị trí tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số '1' được nhóm lại. Ví dụ:```
n = 6, k = 1
s = 011110
```Vì$L = 4$, kiểm tra tham lam đặt một khoảng bao gồm tất cả các khoảng trong một phân đoạn duy nhất, do đó nó trả về giá trị đúng. Nếu như$L$nhỏ hơn thì cần có nhiều phân đoạn, vượt quá$k$. 

Một trường hợp cạnh khác là khi những trường hợp này bị cô lập:```
n = 5, k = 2
s = 10101
```Như được hiển thị trong hướng dẫn, nhỏ$L$buộc một thao tác trên mỗi '1', nhanh chóng vượt quá$k$. Kiểm tra tham lam đếm chính xác từng phân đoạn bắt buộc. 

Trường hợp cạnh cuối cùng là khi$k$đủ lớn để bất kỳ$L=1$hoạt động. Ví dụ:```
n = 10, k = 10
s = 1010101010
```Mỗi số '1' có thể được xử lý riêng lẻ nên câu trả lời sẽ trở thành 1. Thuật toán xử lý việc này một cách tự nhiên vì`can(1)`ngay lập tức trả về đúng.
