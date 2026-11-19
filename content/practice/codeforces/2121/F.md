---
title: "CF 2121F - Yamakasi"
description: "Chúng ta được cung cấp một mảng số nguyên và đối với mỗi trường hợp thử nghiệm, hai giá trị đích: tổng mảng con bắt buộc s và giá trị tối đa bắt buộc x. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề có tổng bằng s và phần tử lớn nhất của nó chính xác là x."
date: "2026-06-08T03:50:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2121
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1032 (Div. 3)"
rating: 1800
weight: 2121
solve_time_s: 181
verified: true
draft: false
---

[CF 2121F - Yamakasi](https://codeforces.com/problemset/problem/2121/F) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên và đối với mỗi trường hợp thử nghiệm, hai giá trị đích: tổng mảng con bắt buộc`s`và giá trị tối đa cần thiết`x`. Nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề có tổng tổng bằng`s`và phần tử lớn nhất của nó chính xác là`x`. 

Một cách hữu ích để nghĩ về điều này là mỗi mảng con đều đóng góp hai ràng buộc độc lập. Điều kiện tổng phụ thuộc vào tất cả các phần tử cùng nhau, trong khi điều kiện tối đa chỉ phụ thuộc vào phần tử lớn nhất bên trong phân đoạn. Một phân đoạn hợp lệ phải đáp ứng cả hai cùng một lúc. 

Các ràng buộc buộc chúng ta phải hành xử tuyến tính hoặc gần tuyến tính. Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó, bất kỳ giải pháp nào tệ hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có nguy cơ hết thời gian chờ. Việc quét bậc hai trên tất cả các mảng con ngay lập tức không khả thi vì nó sẽ kiểm tra tối đa khoảng 10^10 phân đoạn trong trường hợp xấu nhất. 

Một số tình huống khó khăn có thể quan trọng và có thể phá vỡ logic ngây thơ. 

Một vấn đề là khi`x`không xuất hiện trong một phân đoạn nhưng tất cả các phần tử đều nhỏ hơn. Các phân đoạn như vậy có thể thỏa mãn điều kiện tổng nhưng phải bị loại trừ vì mức tối đa của chúng hoàn toàn nhỏ hơn`x`. Ví dụ, nếu`x = 5`và một đoạn là`[1, 2, 3]`, tổng của nó có thể bằng`s`, nhưng nó không hợp lệ. 

Một trường hợp tế nhị khác xảy ra khi các phần tử vượt quá`x`. Bất kỳ mảng con nào chứa phần tử lớn hơn`x`phải được loại trừ hoàn toàn, vì mức tối đa của nó tự động vi phạm yêu cầu. Ví dụ, nếu`x = 3`, thì một đoạn chứa`4`luôn luôn không hợp lệ bất kể tổng của nó là bao nhiêu. 

Cuối cùng, có một sự tương tác không hề nhỏ giữa việc đếm tổng và hạn chế tối đa: việc đếm tổng rất dễ dàng với các tổng tiền tố, nhưng hạn chế tối đa sẽ chia mảng thành các vùng độc lập, việc này phải được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các mảng con`[l, r]`, tính tổng và giá trị lớn nhất của chúng rồi đếm những giá trị phù hợp với cả hai điều kiện. Tổng có thể được tính bằng O(1) bằng cách sử dụng tổng tiền tố, nhưng chi phí tối đa vẫn là O(n) cho mỗi phân đoạn trừ khi được xử lý trước. Ngay cả với quá trình tiền xử lý, việc liệt kê tất cả các phân đoạn O(n^2) sẽ dẫn đến thời gian bậc hai, quá chậm đối với n lên tới 2 · 10^5. 

Quan sát quan trọng là hạn chế tối đa là trình điều khiển cấu trúc thực. Thay vì kiểm tra trực tiếp “max bằng x”, chúng ta có thể phân chia bài toán: 

Một mảng con có giá trị tối đa chính xác`x`nếu và chỉ nếu: 

1. Mỗi phần tử có nhiều nhất`x`và 
2. Nó chứa ít nhất một phần tử bằng`x`. 

Điều này gợi ý việc chia mảng thành các khối tối đa được phân tách bằng các phần tử lớn hơn`x`. Bên trong mỗi khối, tất cả các phần tử đều ≤ x nên điều kiện duy nhất còn lại để xác thực là “chứa ít nhất một x”. 

Bây giờ vấn đề trở thành: trong mỗi khối, đếm các mảng con có tổng`s`bao gồm ít nhất một lần xuất hiện của`x`. 

Chúng tôi xử lý vấn đề này bằng cách sử dụng thủ thuật phân rã tiêu chuẩn: đếm tất cả các mảng con có tổng`s`bên trong khối, sau đó trừ đi những phần không chứa`x`. Mảng con không chứa`x`sống hoàn toàn bên trong các phân đoạn nhỏ hơn được hình thành bằng cách chia khối ở mọi vị trí nơi`a[i] == x`. 

Vì vậy mỗi khối được chia thành các phân đoạn phần tử một cách chặt chẽ`< x`. Đối với mỗi phân đoạn như vậy, chúng tôi đếm các mảng con có tổng`s`sử dụng tổng tiền tố và đếm tần số hashmap. 

Điều này làm giảm vấn đề tổng thể thành hai lần đếm tổng tiền tố trên các phân đoạn tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

### 1. Chia mảng theo phần tử lớn hơn x 

Chúng tôi quét mảng và chia nó thành các phân đoạn liền kề trong đó tất cả các giá trị đều là x. Bất kỳ phân đoạn nào chứa giá trị > x đều không thể đóng góp bất kỳ mảng con hợp lệ nào vượt qua giá trị đó, do đó, nó hoạt động như một ranh giới cứng. 

Điều này đảm bảo chúng tôi chỉ làm việc bên trong các khu vực có khả năng tối đa là x. 

### 2. Bên trong mỗi đoạn hợp lệ, phân cách các vị trí bằng x 

Trong một phân khúc, chúng tôi xác định các chỉ số trong đó`a[i] == x`. Các vị trí này sẽ thực thi yêu cầu “phải chứa x”. 

Về mặt khái niệm, chúng tôi chia phân đoạn thành các phân đoạn nhỏ hơn trong đó tất cả các giá trị hoàn toàn nhỏ hơn x. 

### 3. Đếm tất cả các mảng con có tổng s bên trong đoạn 

Chúng tôi tính toán số mảng con với tổng`s`sử dụng tổng tiền tố. Chúng tôi duy trì một bản đồ băm`freq[prefix_sum]`và với mỗi vị trí, hãy tính xem có bao nhiêu tổng tiền tố trước đó bằng nhau`current_sum - s`. 

Điều này đưa ra tổng số mảng con với tổng`s`, bất kể chúng có chứa x hay không. 

### 4. Trừ mảng con không chứa x 

Một mảng con không có x phải nằm hoàn toàn bên trong một trong các phân đoạn “< x only” được tạo bằng cách cắt ở mỗi lần xuất hiện của x. 

Vì vậy, chúng tôi lặp lại các phân đoạn nhỏ hơn này và đếm lại các mảng con với tổng`s`sử dụng cùng một phương pháp tổng tiền tố, tính tổng trên tất cả các phân đoạn như vậy. 

### 5. Tổng hợp kết quả 

Đối với mỗi phân đoạn hợp lệ: 

result += (đếm tất cả các mảng con tổng-s) − (đếm các mảng con tổng-s không có x) 

### Tại sao nó hoạt động 

Thuật toán dựa trên việc phân vùng không gian tìm kiếm thành các lớp rời rạc. Mọi mảng con trong một vùng mà tất cả các phần tử ≤ x đều bị thiếu x hoàn toàn hoặc chứa ít nhất một x. Hai loại này tách rời nhau và bao gồm tất cả các khả năng. Phương pháp tổng tiền tố đếm chính xác các mảng con bằng cách chuyển đổi ràng buộc tổng thành khớp tần số dựa trên sự khác biệt về tiền tố. Bởi vì mỗi phân đoạn được xử lý độc lập nên không có phân đoạn nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def count_sum(arr, target):
    freq = defaultdict(int)
    freq[0] = 1
    cur = 0
    res = 0
    for v in arr:
        cur += v
        res += freq[cur - target]
        freq[cur] += 1
    return res

def solve():
    t = int(input())
    for _ in range(t):
        n, s, x = map(int, input().split())
        a = list(map(int, input().split()))

        ans = 0
        i = 0

        while i < n:
            if a[i] > x:
                i += 1
                continue

            j = i
            while j < n and a[j] <= x:
                j += 1

            segment = a[i:j]

            total = count_sum(segment, s)

            # subtract subarrays that contain no x
            bad = 0
            k = 0
            while k < len(segment):
                if segment[k] == x:
                    k += 1
                    continue
                l = k
                tmp = []
                while k < len(segment) and segment[k] != x:
                    tmp.append(segment[k])
                    k += 1
                bad += count_sum(tmp, s)

            ans += total - bad
            i = j

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tách biệt các vùng hợp lệ nơi có thể thỏa mãn ràng buộc tối đa. Sau đó, nó đếm tất cả các mảng con với tổng yêu cầu bằng cách sử dụng bản đồ tần số tiền tố. Sau đó, nó loại bỏ những mảng con không bao giờ bao gồm`x`bằng cách tính toán lại quá trình đếm tương tự trên các phân đoạn được phân tách bằng số lần xuất hiện của`x`. 

Một chi tiết tinh tế là chúng tôi không bao giờ kiểm tra rõ ràng các hàm đếm bên trong tối đa. Ràng buộc tối đa được thực thi theo cấu trúc bằng cách phân đoạn thay vì được tính toán trực tiếp, điều này tránh mọi truy vấn tối đa O(n) trên mỗi mảng con. 

## Ví dụ đã hoạt động 

Hãy xem xét một dấu vết đơn giản hóa: 

đầu vào:```
1
5 3 2
1 2 -1 2 1
```Chúng tôi xử lý phân đoạn đơn vì tất cả các giá trị đều 2. 

| Bước | Phân đoạn | Hành động hiện tại | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | [1,2,-1,2,1] | mảng con tổng cộng | tất cả các ứng viên hợp lệ bỏ qua max | 
| 2 | chia tại x=2 | [1], [2], [-1], [2], [1] | cô lập các vùng không có x | 
| 3 | tính xấu | mảng con không có 2 | chỉ trong [1], [-1], [1] | 
| 4 | trả lời | tổng - xấu | chỉ giữ các mảng con chứa 2 | 

Điều này cho thấy phép trừ thực thi chính xác “phải bao gồm x”. 

Bây giờ hãy xem xét trường hợp có giá trị chặn:```
1
5 0 2
2 1 5 1 2
```| Bước | Phân đoạn | Hành động | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | chia cho >2 | [2,1], [1,2] | 5 đoạn nghỉ | 
| 2 | quá trình [2,1] | tổng tiền tố | vùng hợp lệ | 
| 3 | quá trình [1,2] | tổng tiền tố | vùng độc lập | 

Điều này khẳng định rằng các phần tử lớn hơn`x`tách biệt chính xác các tính toán độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử tham gia vào một số lần quét tổng tiền tố không đổi trên toàn bộ phân đoạn | 
| Không gian | O(n) | Hashmap cho tần số tiền tố trong các phân đoạn | 

Tổng chiều dài mảng trong tất cả các trường hợp thử nghiệm được giới hạn bởi 2 · 10^5, do đó việc xử lý tuyến tính trên mỗi phần tử dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bản đồ băm tiền tố đảm bảo hoạt động khấu hao liên tục trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# NOTE: placeholder since full solution function not wrapped
# These are structural tests rather than executable asserts

# edge: single element equal to x and s
# edge: all elements > x
# edge: negative numbers with valid sum
# edge: multiple blocks split by > x
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp phần tử đơn | 1 | trường hợp tối thiểu | 
| tất cả > x | 0 | từ chối hoàn toàn | 
| số tiền âm | đếm đúng | tính chính xác của tổng tiền tố | 
| nhiều khối | câu trả lời kết hợp | độ chính xác của phân đoạn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mọi phần tử đều lớn hơn`x`. Trong tình huống này, thuật toán ngay lập tức bỏ qua tất cả các phân đoạn do không có khối hợp lệ nào được hình thành. Ví dụ: 

đầu vào:```
1
4 5 2
3 4 5 6
```Quá trình quét không tìm thấy đoạn nào ở đó`a[i] <= 2`, do đó câu trả lời vẫn là 0. Điều này đúng vì không có phân mảng nào có thể có tối đa chính xác là 2. 

Một trường hợp cạnh khác xảy ra khi`x`xuất hiện nhiều lần nhưng các mảng con hợp lệ tổng không bao giờ bao gồm nó. Trong trường hợp đó,`total - bad`tự nhiên trở thành 0 vì tất cả các phân đoạn có tổng hợp lệ đều nằm trong vùng không có x.
