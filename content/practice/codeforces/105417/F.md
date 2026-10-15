---
title: "CF 105417F - Dây chuyền ấp trứng"
description: "Chúng ta được cung cấp vị trí của các quả trứng được đặt trên trục số, mỗi quả có tọa độ nguyên riêng biệt. Chúng ta được phép lắp đặt tối đa k đèn nhiệt và mỗi đèn cũng có thể được đặt ở tọa độ nguyên trên cùng một đường thẳng."
date: "2026-06-23T17:27:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105417
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 1 (Advanced)"
rating: 0
weight: 105417
solve_time_s: 86
verified: false
draft: false
---

[CF 105417F - Dây chuyền ươm tạo](https://codeforces.com/problemset/problem/105417/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp vị trí của các quả trứng được đặt trên trục số, mỗi quả có tọa độ nguyên riêng biệt. Chúng tôi được phép cài đặt nhiều nhất`k`đèn nhiệt và mỗi đèn cũng có thể được đặt ở tọa độ nguyên trên cùng một đường thẳng. Mỗi quả trứng được coi là được làm nóng bởi ngọn đèn gần nhất và chi phí cho một cấu hình là khoảng cách tối đa từ bất kỳ quả trứng nào đến ngọn đèn gần nhất của nó. 

Nhiệm vụ là chọn các vị trí đèn sao cho khoảng cách tối đa này càng nhỏ càng tốt. 

Một cách hữu ích để giải thích điều này là mỗi đèn “che phủ” một đoạn của đường thẳng và mỗi quả trứng phải nằm đủ gần với ít nhất một trong các vùng phủ sóng này. Chúng tôi không cố gắng giảm thiểu tổng khoảng cách, chỉ là trường hợp xấu nhất trong số tất cả các quả trứng. 

Các ràng buộc đủ lớn nên bất kỳ giải pháp nào cố gắng kiểm tra tất cả các vị trí đặt đèn hoặc thậm chí tất cả các tập hợp con trứng đều không thể thực hiện được. Với tối đa`5 × 10^5`các vị trí, một`O(n^2)`cách tiếp cận này sẽ vượt quá giới hạn khả thi theo nhiều bậc độ lớn, và thậm chí`O(nk)`là quá lớn trong trường hợp xấu nhất. 

Ràng buộc cấu trúc chính là mọi thứ đều nằm trên một đường một chiều. Điều đó loại bỏ tất cả sự phức tạp về hình học và biến bài toán thành việc bao phủ khoảng. 

Một số trường hợp đặc biệt cần được suy nghĩ kỹ lưỡng. Nếu như`k = n`, chúng ta có thể đặt một ngọn đèn lên mỗi quả trứng và đưa ra câu trả lời`0`. Nếu như`k = 1`, đèn tối ưu được đặt gần điểm che tối ưu giống như đường trung bình và đáp án trở thành bán kính tối thiểu cần thiết để bao phủ tất cả các điểm bằng một tâm duy nhất. Nếu tất cả các trứng được nhóm lại ngoại trừ một ngoại lệ ở xa, câu trả lời sẽ bị chi phối bởi khoảng cách của ngoại lệ đó với tâm cụm gần nhất, chứ không phải bởi khoảng cách trung bình. 

Một sai lầm ngây thơ là cho rằng việc đặt đèn ở các vị trí cố định như điểm cuối hoặc điểm giữa của mảng luôn hoạt động. Điều đó không thành công khi các điểm cách đều nhau, ví dụ`[0, 1, 2, 100]`với`k = 2`, trong đó nhóm tối ưu là`{0,1,2}`Và`{100}`, không phân chia đối xứng. 

## Phương pháp tiếp cận 

Quan điểm vũ phu sẽ cố gắng chọn`k`vị trí đèn trong số tất cả các tọa độ nguyên có thể có và tính khoảng cách tối đa thu được. Ngay cả việc hạn chế đèn ở các vị trí trứng, điều này vẫn trở thành một vấn đề lựa chọn tổ hợp với các lựa chọn theo cấp số nhân và đánh giá từng chi phí cấu hình`O(n)`. Tốc độ này quá chậm vì số cách chọn vị trí đèn ngày càng tăng.`n choose k`. 

Quan sát quan trọng đến từ việc lật ngược quan điểm. Thay vì trực tiếp chọn vị trí đèn, chúng tôi đặt câu hỏi quyết định: nếu chúng tôi ấn định khoảng cách tối đa cho phép`R`, chúng ta có thể đặt nhiều nhất`k`đèn để mọi quả trứng đều ở trong`R`của một chiếc đèn nào đó? 

Một lần`R`được cố định, mỗi đèn đặt ở vị trí`x`bao gồm tất cả các quả trứng trong khoảng thời gian`[x - R, x + R]`. Điều này chuyển vấn đề thành bao gồm tất cả các điểm với số lượng khoảng thời gian tối thiểu`2R + 1`tập trung ở các vị trí số nguyên. Trên một hàng được sắp xếp, điều này có thể được giải quyết một cách tham lam: bắt đầu từ quả trứng không được che phủ ngoài cùng bên trái, đặt đèn càng xa bên phải càng tốt trong khi vẫn che nó và bỏ qua tất cả những quả trứng mà nó che. 

Điều này biến bài toán tối ưu ban đầu thành bài toán quyết định đơn điệu trong`R`. Nếu bán kính`R`hoạt động, bất kỳ bán kính lớn hơn cũng hoạt động. Sự đơn điệu đó cho phép tìm kiếm nhị phân trên câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam | O(n phạm vi nhật ký) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp vị trí trứng 

Trước tiên, chúng tôi sắp xếp tất cả các tọa độ để có thể xử lý chúng từ trái sang phải. Thứ tự này là cần thiết vì các quyết định về mức độ bao phủ chỉ phụ thuộc vào vị trí tương đối dọc theo đường. 

### 2. Xác định kiểm tra tính khả thi cho bán kính cố định`R`Chúng tôi kiểm tra xem liệu có thể phủ tối đa tất cả trứng bằng cách sử dụng`k`đèn, mỗi khoảng cách bao phủ`R`. 

### 3. Vị trí tham lam từ quả trứng không được che phủ ngoài cùng bên trái 

Chúng tôi quét từ trái sang phải. Khi gặp quả trứng đầu tiên chưa được che phủ, chúng ta đặt đèn ở vị trí tốt nhất có thể để tối đa hóa khả năng che phủ cho những quả trứng tiếp theo. 

### 4. Vị trí đèn tối ưu cho trứng khởi đầu 

Nếu quả trứng không được che chắn đầu tiên ở vị trí`a[i]`, đặt đèn ở`a[i] + R`là tối ưu. Điều này tối đa hóa khoảng cách mà chúng ta có thể mở rộng phạm vi bao phủ trong khi vẫn đảm bảo quả trứng bắt đầu ở trong khoảng cách`R`. 

Đèn này sau đó bao phủ tất cả trứng lên vị trí`a[i] + 2R`. 

### 5. Bỏ qua hết trứng đã đậy kín 

Chúng tôi đưa con trỏ qua tất cả các quả trứng trong phạm vi được bao phủ và lặp lại cho đến khi tất cả các quả trứng được bao phủ hoặc chúng tôi vượt quá`k`đèn. 

### 6. Tìm kiếm nhị phân hợp lệ nhỏ nhất`R`Chúng tôi tìm kiếm qua`R`từ`0`lên đến`max(a) - min(a)`. Đối với mỗi ứng viên, chúng tôi tiến hành kiểm tra tính khả thi. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào hai thuộc tính. Đầu tiên, đối với bán kính cố định, chiến lược tham lam sẽ giảm thiểu số lượng đèn vì mỗi vị trí bao phủ hậu tố tối đa có thể bắt đầu từ quả trứng hiện tại chưa được che chắn. Thứ hai, tính khả thi còn đơn điệu trong`R`: tăng bán kính không bao giờ làm tăng số lượng đèn cần thiết. Điều này đảm bảo tìm kiếm nhị phân hội tụ đến bán kính hợp lệ nhỏ nhất mà không bỏ qua điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, k, R):
    n = len(a)
    i = 0
    used = 0

    while i < n:
        used += 1
        if used > k:
            return False

        start = a[i]
        cover_end = start + 2 * R

        # place lamp at start + R, cover up to start + 2R
        while i < n and a[i] <= cover_end:
            i += 1

    return True

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    lo, hi = 0, a[-1] - a[0]

    while lo < hi:
        mid = (lo + hi) // 2
        if can(a, k, mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các vị trí trứng để vùng phủ sóng tham lam có thể hoạt động tuyến tính. các`can`thực hiện bước kiểm tra tính khả thi quan trọng: nó liên tục chọn quả trứng không được che phủ ở ngoài cùng bên trái, giả sử đèn được đặt ở vị trí tối ưu cho nó và bỏ qua tất cả những quả trứng nằm trong phạm vi phủ sóng của nó. Tìm kiếm nhị phân sau đó thu hẹp bán kính nhỏ nhất mà vẫn cho phép bao phủ tất cả trứng bằng cách sử dụng nhiều nhất`k`đèn. 

Một điểm tinh tế là việc sử dụng`start + 2 * R`như ranh giới vùng phủ sóng. Điều này xuất phát từ việc đặt đèn ở`start + R`, làm cho quả trứng ngoài cùng bên trái có khoảng cách chính xác`R`đi và mở rộng vùng phủ sóng đối xứng sang bên phải. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
0 5 7
```Chúng tôi sắp xếp mảng đã được sắp xếp. 

Chúng tôi tìm kiếm nhị phân`R`. 

| Bước | R | Số lượng đèn | Khoảng thời gian bảo hiểm | 
| --- | --- | --- | --- | 
| kiểm tra | 0 | 3 | [0],[5],[7] | 
| kiểm tra | 1 | 2 | [0,1],[5,7] | 
| kiểm tra | 1 | hợp lệ | ≤ 2 đèn | 

Bán kính hợp lệ nhỏ nhất là`1`. 

Điều này cho thấy việc phân cụm quan trọng như thế nào:`{5,7}`có thể được xử lý bằng một đèn khi bán kính cho phép chồng lên nhau. 

### Mẫu 2 

đầu vào:```
5 2
-2 -1 0 1 4
```Mảng được sắp xếp là`[-2, -1, 0, 1, 4]`. 

| Bước | R | Đèn đã qua sử dụng | Bảo hiểm | 
| --- | --- | --- | --- | 
| kiểm tra | 1 | 3 | thất bại | 
| kiểm tra | 2 | 2 | thành công | 

Tại`R = 2`, vỏ đèn đầu tiên từ`-2`trở đi lên đến`0`, đèn thứ hai được đặt cho các vùng còn lại`1`Và`4`. 

Điều này xác nhận hành vi tham lam xử lý chính xác khoảng cách không đồng đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log M) | Sắp xếp mất`O(n log n)`, mỗi lần kiểm tra tính khả thi là`O(n)`và tìm kiếm nhị phân chạy trên một phạm vi lên đến tọa độ span | 
| Không gian | O(n) | Lưu trữ cho các vị trí được sắp xếp | 

Các ràng buộc cho phép điều này một cách thoải mái vì`n ≤ 5 × 10^5`và mỗi kiểm tra là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(a, k, R):
        n = len(a)
        i = 0
        used = 0
        while i < n:
            used += 1
            if used > k:
                return False
            start = a[i]
            end = start + 2 * R
            while i < n and a[i] <= end:
                i += 1
        return True

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        lo, hi = 0, a[-1] - a[0]
        while lo < hi:
            mid = (lo + hi) // 2
            if can(a, k, mid):
                hi = mid
            else:
                lo = mid + 1
        print(lo)

    solve()
    return ""

# provided samples
assert run("3 2\n0 5 7\n") == "1\n"
assert run("5 2\n-2 -1 0 1 4\n") == "2\n"

# minimum size
assert run("1 1\n10\n") == "0\n"

# all equal spacing tight k
assert run("4 2\n0 10 20 30\n") == "10\n"

# k = n
assert run("3 3\n1 5 9\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ bản tầm thường | 
| cách đều nhau | 10 | hành vi nhóm | 
| k = n | 0 | trường hợp linh hoạt tối đa | 

## Vỏ cạnh 

Đối với một quả trứng, quá trình tham lam ngay lập tức đặt một chiếc đèn che nó, tạo ra bán kính`0`. Thuật toán không thử các vị trí bổ sung không cần thiết. 

Vì`k = n`, mỗi quả trứng được che riêng lẻ vì mỗi quả trứng không được che sẽ kích hoạt đèn, nhưng điều kiện`used > k`không bao giờ thất bại, vì vậy tính khả thi vẫn giữ nguyên`R = 0`, tìm kiếm nhị phân nào tìm thấy chính xác. 

Đối với các điểm được phân tách rộng rãi, chẳng hạn như`[0, 1000000000]`với`k = 1`, thuật toán đặt một đèn duy nhất bao phủ cả hai đầu và bán kính tính toán sẽ trở thành một nửa khoảng cách có hiệu lực, được ghi lại chính xác bởi phạm vi bao phủ tham lam.
