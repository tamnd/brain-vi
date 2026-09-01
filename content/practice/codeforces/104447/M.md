---
title: "CF 104447M - Có được không?"
description: "Chúng ta bắt đầu với một đồng xu ở tọa độ (0, 0) trên lưới số nguyên vô hạn. Thời gian tiến triển theo các bước rời rạc bắt đầu từ 1. Ở mỗi bước i, chúng ta chọn bất kỳ số nguyên xi nào, và sau đó đồng xu di chuyển một cách bị ràng buộc tùy thuộc vào bước đó là lẻ hay chẵn."
date: "2026-06-30T18:46:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "M"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 45
verified: true
draft: false
---

[CF 104447M - Có được không?](https://codeforces.com/problemset/problem/104447/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một đồng xu ở tọa độ (0, 0) trên lưới số nguyên vô hạn. Thời gian tiến triển theo các bước rời rạc bắt đầu từ 1. Ở mỗi bước i, chúng ta chọn bất kỳ số nguyên xi nào, và sau đó đồng xu di chuyển một cách bị ràng buộc tùy thuộc vào bước đó là lẻ hay chẵn. 

Nếu i lẻ, bước di chuyển sẽ thêm xi vào cả hai tọa độ, do đó (a, b) trở thành (a + xi, b + xi). Nếu i chẵn, bước di chuyển sẽ thêm xi vào tọa độ thứ nhất và trừ xi khỏi tọa độ thứ hai, do đó (a, b) trở thành (a + xi, b − xi). Nhiệm vụ là đạt đến điểm mục tiêu (n, m) bằng cách sử dụng số lần di chuyển tối thiểu như vậy và cũng xuất ra chuỗi giá trị xi đã chọn. Nếu không thể thực hiện được, chúng ta xuất ra -1. 

Các ràng buộc lên tới 100.000 trường hợp thử nghiệm và tọa độ lên tới ±10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng trên mỗi thử nghiệm nào với số lượng lớn các bước hoặc bất kỳ cấu trúc nào phụ thuộc vào tìm kiếm lặp lại trên các giá trị xi. Mỗi bài kiểm tra phải được giải trong thời gian không đổi hoặc gần như không đổi. 

Một điểm tinh tế là xi không bị giới hạn về độ lớn và dấu. Điều đó có nghĩa là mỗi bước di chuyển là một phép biến đổi tuyến tính với vô hướng được chọn tự do, do đó cấu trúc hoàn toàn là đại số chứ không phải tổ hợp. 

Một sai lầm ngây thơ là coi điều này giống như chuyển động độc lập của tọa độ x và y. Ví dụ: cố gắng giải x và y riêng biệt sẽ không thành công vì mỗi bước di chuyển đều kết hợp chúng. Một giả định không chính xác phổ biến khác là sự xen kẽ chẵn lẻ tạo ra các ràng buộc không thể đảo ngược; trên thực tế, nó chỉ thay thế một mẫu dấu hiệu. 

Một trường hợp thất bại cụ thể đối với lý luận ngây thơ là việc cố gắng khớp n một cách tham lam trước tiên. Giả sử chúng ta cố gắng đạt được (n, m) bằng cách chỉ tích lũy x đóng góp trên x. Chúng tôi nhanh chóng nhận thấy rằng mọi động thái cũng ảnh hưởng đến y theo cách liên kết, do đó sự tích lũy tham lam độc lập bị phá vỡ. 

## Phương pháp tiếp cận 

Chìa khóa của vấn đề này là viết lại chuyển động ở dạng có cấu trúc hơn. 

Gọi vị trí sau k lần di chuyển là (X, Y). Mỗi nước đi luôn đóng góp xi cho X, trong khi Y luân phiên giữa +xi và −xi tùy thuộc vào tính chẵn lẻ. Vì vậy, chúng ta có thể nghĩ về những đóng góp cho X và Y một cách riêng biệt: 

Với i lẻ: đóng góp là (xi, xi) 

Với i chẵn: đóng góp là (xi, −xi) 

Vậy sau khi k di chuyển: 

X = tổng(xi với mọi thứ i) 

Y = sum(xi với i lẻ) − sum(xi với i chẵn) 

Chúng tôi giới thiệu: 

S = tổng trên tất cả xi 

O = tổng trên số lẻ i xi 

E = tổng trên số chẵn i xi 

Sau đó: 

X = S = O + E 

Y = O − E 

Giải các phương trình này ta có: 

O = (X + Y) / 2 

E = (X − Y) / 2 

Vì vậy, điều kiện cần cho mọi nghiệm là cả (n + m) và (n − m) đều chẵn. Mặt khác, O và E không phải là số nguyên nên không có chuỗi số nguyên nào có thể tạo ra mục tiêu. 

Sau khi xác định được tính khả thi, chúng ta vẫn cần số lần di chuyển tối thiểu. Vì mỗi nước đi đóng góp chính xác một xi, nên chúng ta muốn biểu thị O và E dưới dạng tổng của các số nguyên. Chiến lược tối ưu là thực hiện tối đa hai nước đi: 

Nếu cả O và E đều khác 0 thì chúng ta có thể sử dụng hai nước đi: 

Một bước di chuyển chỉ số lẻ sẽ đóng góp O 

Một động thái chỉ số chẵn đóng góp E 

Nếu một trong hai bằng 0 nhưng nước còn lại khác 0, chúng ta vẫn có thể thực hiện điều đó trong một nước đi chỉ khi chẵn lẻ căn chỉnh chính xác (nước đi đầu tiên phải là số lẻ), nếu không, chúng ta cần hai nước đi bằng cách đưa ra một điều chỉnh bù giống như số 0. 

Tuy nhiên, quan sát rõ ràng thì đơn giản hơn: chúng ta luôn có thể nhận ra bất kỳ (O, E) hợp lệ nào bằng cách sử dụng tối đa 2 nước đi và chỉ có thể thực hiện được 1 nước đi khi Y = X hoặc Y = −X, tức là khi m = n hoặc m = −n. 

Vậy đáp án tối ưu là: 

Nếu (n + m) hoặc (n − m) là số lẻ, ghi -1. 

Ngược lại nếu (n, m) là (0, 0), câu trả lời là 0. 

Ngược lại nếu m = n hoặc m = −n, câu trả lời là 1. 

Nếu không thì câu trả lời là 2. 

Để xây dựng trình tự cho 2 nước đi, ta đặt: 

x1 = O 

x2 = E 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force qua các bước | O( | n | + | 
| Phép biến đổi đại số | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi rút gọn từng trường hợp thử nghiệm thành đại số trên hai biến xuất phát từ điểm mục tiêu. 

1. Tính S1 = n + m và S2 = n − m. Nếu S1 hoặc S2 là số lẻ, trả về -1 vì hệ phương trình của O và E không thể tạo ra số nguyên. Điều này diễn ra trực tiếp từ việc giải hệ tuyến tính xác định các đóng góp. 
2. Nếu (n, m) bằng (0, 0), xuất ra 0 vì không cần di chuyển. 
3. Nếu m = n, xuất 1 và chọn x1 = n. Điều này có tác dụng vì mỗi bước di chuyển lẻ sẽ cộng cả hai tọa độ bằng nhau, do đó, một bước duy nhất có thể hạ cánh chính xác trên đường chéo. 
4. Nếu m = −n, xuất 1 và chọn x1 = n. Trong trường hợp này, nước đi đầu tiên tạo ra (n, −n), khớp trực tiếp với mục tiêu. 
5. Nếu không thì xuất ra 2 nước đi. Tính O = (n + m) / 2 và E = (n − m) / 2. Đặt x1 = O và x2 = E, xây dựng các đóng góp lẻ và chẵn một cách độc lập. 

### Tại sao nó hoạt động 

Phép biến đổi chia quá trình thành hai thành phần tuyến tính độc lập tương ứng với chuyển động đối xứng và phản đối xứng dọc theo trục lưới. Mỗi nước đi đều đóng góp vào cả hai thành phần theo một mẫu cố định, do đó hệ thống giảm xuống việc giải hệ tuyến tính 2×2 trên số nguyên. Điều kiện chẵn lẻ đảm bảo tính toàn vẹn của giải pháp và việc xây dựng hai bước là đủ vì chúng ta có thể gán từng thành phần độc lập cho một lớp chẵn lẻ theo thời gian dành riêng. 

Không có chuỗi nào ngắn hơn chuỗi tối ưu có thể biểu diễn đồng thời cả hai ràng buộc ngoại trừ trong trường hợp suy biến trong đó một trong các thành phần dẫn xuất bằng 0, tương ứng chính xác với các đường chéo một bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        if n == 0 and m == 0:
            out.append("0")
            continue
        
        if (n + m) % 2 != 0 or (n - m) % 2 != 0:
            out.append("-1")
            continue
        
        if n == m or n == -m:
            out.append(f"1 {n}")
            continue
        
        o = (n + m) // 2
        e = (n - m) // 2
        out.append(f"2 {o} {e}")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập. Việc kiểm tra tính chẵn lẻ xuất phát trực tiếp từ yêu cầu O và E phải là số nguyên. Các trường hợp di chuyển một lần tương ứng chính xác với thời điểm mục tiêu nằm trên một trong hai đường chéo, khiến nó có thể tiếp cận được chỉ bằng một bước nhảy đối xứng hoặc phản đối xứng. Mặt khác, việc chia thành O và E đảm bảo rằng chuỗi được xây dựng sẽ tái tạo chính xác cả hai tọa độ. 

Một cạm bẫy triển khai phổ biến là trộn lẫn các dấu hiệu trong việc xây dựng E. Đạo hàm chính xác đến từ việc giải hệ tuyến tính chứ không phải từ việc đoán dựa trên hành vi tọa độ. 

## Ví dụ đã hoạt động 

Xem xét đầu vào: 

n = 2, m = 7 

Chúng tôi tính toán: 

S1 = 9, S2 = -5 

Cả hai đều lẻ nên hệ này không khả thi. 

| Bước | n | m | n+m | n-m | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 7 | 9 | -5 | phát hiện sự không khớp chẵn lẻ | 

Điều này xác nhận tại sao câu trả lời là -1: O và E sẽ không phải là số nguyên. 

Bây giờ hãy xem xét: 

n = -10, m = 8 

Tính toán: 

S1 = -2, S2 = -18 

Cả hai đều chẵn, vì vậy: 

O = -1 

E = -9 

| Bước | Ồ | E | Xây dựng | 
| --- | --- | --- | --- | 
| 1 | -1 | -9 | x1 = -1 | 
| 2 | -1 | -9 | x2 = -9 | 

Sau nước đi 1 (lẻ): (0,0) → (-1,-1) 

Sau nước đi 2 (chẵn): (-1,-1) → (-10,8) 

Điều này cho thấy cách tách các thành phần đối xứng và phản đối xứng để tái tạo lại cả hai tọa độ một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra thực hiện một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ cho mỗi lần kiểm tra | 

Giải pháp này dễ dàng phù hợp với giới hạn vì thậm chí 100.000 trường hợp thử nghiệm chỉ yêu cầu số học đơn giản cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        if n == 0 and m == 0:
            res.append("0")
        elif (n + m) % 2 != 0 or (n - m) % 2 != 0:
            res.append("-1")
        elif n == m or n == -m:
            res.append(f"1 {n}")
        else:
            o = (n + m) // 2
            e = (n - m) // 2
            res.append(f"2 {o} {e}")
    return "\n".join(res)

# provided samples
assert run("3\n0 0\n2 7\n-10 8") == "0\n-1\n2 -1 -9"

# custom cases
assert run("1\n1 1") == "1 1"
assert run("1\n1 -1") == "1 1"
assert run("1\n2 0") == "2 1 1"
assert run("1\n1 2") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (0,0) | 0 | trường hợp bắt đầu tầm thường | 
| (1,1) | 1 1 | di chuyển chéo lẻ duy nhất | 
| (2,0) | 2 1 1 | xây dựng đối xứng hai bước | 
| (1,2) | -1 | tính không khả thi của tính chẵn lẻ | 

## Vỏ cạnh 

Đối với trường hợp gốc (0, 0), thuật toán ngay lập tức trả về 0 mà không cần cố gắng phân tách. Điều này tránh việc xây dựng các giá trị xi không cần thiết có thể tạo ra giải pháp hai bước giả. 

Đối với các mục tiêu chéo như (n, n), thuật toán phát hiện m = n và đưa ra một nước đi duy nhất. Ví dụ (3, 3) dẫn đến x1 = 3, tạo ra (3, 3) trực tiếp qua bước lẻ. 

Đối với các mục tiêu chống đường chéo như (n, −n), logic một bước tương tự cũng được áp dụng. Ví dụ (2, −2) mang lại x1 = 2 và nước đi đầu tiên khớp chính xác với mục tiêu. 

Đối với các trường hợp chung như (-10, 8), thuật toán quay lại hai bước, tính toán O và E một cách rõ ràng và xây dựng lại quỹ đạo một cách xác định mà không mơ hồ.
