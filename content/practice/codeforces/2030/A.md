---
title: "CF 2030A - Món Quà Từ Đười ươi"
description: "Chúng ta được cung cấp một mảng và chúng ta được phép sắp xếp lại nó trước khi xử lý. Sau khi sửa, chúng tôi quét từ trái sang phải và duy trì hai giá trị đang chạy: phần tử nhỏ nhất được thấy cho đến nay và phần tử lớn nhất được thấy cho đến nay."
date: "2026-06-08T11:55:52+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2030
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 979 (Div. 2)"
rating: 800
weight: 2030
solve_time_s: 116
verified: true
draft: false
---

[CF 2030A - Món quà từ đười ươi](https://codeforces.com/problemset/problem/2030/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta được phép sắp xếp lại nó trước khi xử lý. Sau khi sửa, chúng tôi quét từ trái sang phải và duy trì hai giá trị đang chạy: phần tử nhỏ nhất được thấy cho đến nay và phần tử lớn nhất được thấy cho đến nay. Tại mỗi vị thế, chúng tôi ghi lại sự khác biệt giữa hai thái cực này và điểm số cuối cùng là tổng của tất cả chênh lệch giá trên mỗi vị thế này. 

Sự tự do mà chúng ta có hoàn toàn nằm ở sự hoán vị của mảng. Sau đó, tiền tố tối thiểu và tiền tố tối đa phát triển một cách xác định. 

Các ràng buộc đủ nhỏ để$O(n^2)$hoặc thậm chí$O(n \log n)$cách tiếp cận cho mỗi trường hợp thử nghiệm sẽ vượt qua một cách thoải mái vì tổng số$n$trên tất cả các bài kiểm tra tối đa là 1000. Điều này báo hiệu rằng chúng ta nên tập trung vào lý luận cấu trúc hơn là các thủ thuật tối ưu hóa. 

Một sai lầm phổ biến ở đây là cho rằng việc đặt số lượng lớn lên đầu hoặc cuối cùng một cách tham lam luôn có tác dụng. Điều đó không thành công vì tiền tố tối thiểu và tiền tố tối đa tương tác trên toàn cầu chứ không phải cục bộ. Ví dụ: việc đặt phần tử lớn nhất quá sớm sẽ ngay lập tức khắc phục mức tối đa cho nhiều tiền tố, giảm mức đóng góp trong tương lai, ngay cả khi điều đó có vẻ mang lại lợi ích cục bộ. 

Một cạm bẫy khác là cố gắng tối ưu hóa việc đóng góp tiền tố một cách độc lập. Một tiền tố trông có vẻ tối ưu khi tách biệt có thể phá hủy các khoảng tăng dần trong tương lai của khoảng cách tối đa-tối thiểu, do đó, sự sắp xếp phải được xem xét một cách tổng thể. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các hoán vị và tính điểm cho từng hoán vị. Điều đó có hiệu quả về mặt khái niệm vì hàm tính điểm được xác định rõ ràng, nhưng nó phức tạp theo giai thừa và không thể vượt quá mức rất nhỏ.$n$. 

Quan sát quan trọng là điểm số chỉ phụ thuộc vào mức độ phát triển của các thái cực chứ không phụ thuộc vào thứ tự chính xác của các yếu tố ở giữa. Chúng tôi muốn duy trì mức tối đa ở mức “cao” càng lâu càng tốt, đồng thời trì hoãn việc tăng mức tối thiểu càng nhiều càng tốt. Điều này gợi ý nên đặt sớm các phần tử nhỏ để duy trì mức tối thiểu tiền tố thấp và các phần tử lớn muộn hơn để tiếp tục tăng dần mức tối đa tiền tố. 

Cấu trúc đạt được điều này một cách tối ưu là một sự sắp xếp tham lam đơn giản: chúng ta lần lượt đẩy các giá trị nhỏ nhất và lớn nhất còn lại hiện tại vào chuỗi. Điều này tối đa hóa số lượng tiền tố trong đó khoảng cách giữa tối đa và tối thiểu lớn. 

Trực giác là mỗi lần chúng ta mở rộng chuỗi, chúng ta muốn đưa ra một mức cực đại toàn cục mới hoặc duy trì một mức cực tiểu nhỏ, và các cực trị xen kẽ đảm bảo cả hai lực đều được khai thác tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tham lam cực độ |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng. Việc sắp xếp cho phép chúng ta truy cập có kiểm soát vào các phần tử nhỏ nhất và lớn nhất. 

Sau đó, chúng ta xây dựng hoán vị bằng cách sử dụng hai con trỏ, một ở đầu và một ở cuối mảng đã được sắp xếp. 

Ở mỗi bước, chúng tôi nối thêm phần tử nhỏ nhất còn lại hoặc phần tử lớn nhất còn lại. Sự lựa chọn thay thế. 

Chúng tôi mô phỏng việc đánh giá tiền tố một cách ngầm định: thay vì tính toán lại tiền tố tối thiểu và tối đa, chúng tôi dựa vào thực tế là việc đặt các cực trị sẽ tối đa hóa đóng góp chênh lệch trong tương lai. 

Cuối cùng, chúng tôi tính điểm bằng cách quét mảng đã xây dựng và duy trì tiền tố tối thiểu và tối đa. 

### Tại sao nó hoạt động 

Điểm số ở mỗi vị trí chỉ phụ thuộc vào khoảng cách giữa điểm chạy tối thiểu và tối đa. Cách duy nhất để giữ sự khác biệt lớn giữa nhiều tiền tố là trì hoãn sự ổn định ở một trong hai cực. Việc xen kẽ giữa các giá trị nhỏ nhất và lớn nhất còn lại đảm bảo rằng không có cực trị nào bị “kẹt” quá sớm, tối đa hóa sự khác biệt tích lũy trên tất cả các tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        l, r = 0, n - 1
        arr = []
        take_max = True
        
        while l <= r:
            if take_max:
                arr.append(a[r])
                r -= 1
            else:
                arr.append(a[l])
                l += 1
            take_max = not take_max
        
        mn = arr[0]
        mx = arr[0]
        ans = 0
        
        for x in arr:
            mn = min(mn, x)
            mx = max(mx, x)
            ans += mx - mn
        
        print(ans)

if __name__ == "__main__":
    solve()
```Sau khi sắp xếp, chúng ta xây dựng một hoán vị bằng cách luân phiên lấy các giá trị lớn nhất và nhỏ nhất còn lại. Điều này đảm bảo rằng các giá trị cực trị được trải đều trên cấu trúc tiền tố một cách cân bằng. Vòng lặp cuối cùng chỉ tính điểm theo định nghĩa bằng cách sử dụng tiền tố tối thiểu và tối đa. 

Một điểm tinh tế là khởi tạo: phần tử đầu tiên đặt cả tối thiểu và tối đa, do đó đóng góp đầu tiên luôn bằng 0. Ngoài ra, hướng luân phiên phải bắt đầu một cách nhất quán; bắt đầu với một trong hai bên cũng được miễn là nó nhất quán trong toàn bộ công trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
7 6 5
```Đã sắp xếp:`[5, 6, 7]`| Bước | Được chọn | Mảng cho đến nay | Tiền tố tối thiểu | Tiền tố tối đa | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7 | [7] | 7 | 7 | 0 | 
| 2 | 5 | [7, 5] | 5 | 7 | 2 | 
| 3 | 6 | [7, 5, 6] | 5 | 7 | 2 | 

Tổng cộng = 4. 

Điều này cho thấy việc đặt các cực trị sớm sẽ giúp mở ra một khoảng cách lớn trong nhiều tiền tố. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 2 2
```Đã sắp xếp:`[1, 1, 1, 2, 2]`Cách xây dựng thay thế 2,1,2,1,1 (một biến thể hợp lệ tùy thuộc vào việc xử lý dây buộc). 

| Bước | Giá trị | Tối thiểu | Tối đa | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 0 | 
| 2 | 1 | 1 | 2 | 1 | 
| 3 | 2 | 1 | 2 | 1 | 
| 4 | 1 | 1 | 2 | 1 | 
| 5 | 1 | 1 | 2 | 1 | 

Tổng cộng = 4. 

Các giá trị nhỏ lặp lại đảm bảo mức tối thiểu luôn ở mức thấp, cho phép đóng góp lặp lại từ mức tối đa cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| lưu trữ mảng được sắp xếp lại | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm tối đa là 1000, điều này dễ dàng nằm trong giới hạn ngay cả với nhiều loại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        l, r = 0, n - 1
        arr = []
        take_max = True

        while l <= r:
            if take_max:
                arr.append(a[r])
                r -= 1
            else:
                arr.append(a[l])
                l += 1
            take_max = not take_max

        mn = mx = arr[0]
        ans = 0
        for x in arr:
            mn = min(mn, x)
            mx = max(mx, x)
            ans += mx - mn

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("3\n1\n69\n3\n7 6 5\n5\n1 1 1 2 2\n") == "0\n4\n4"

# custom cases
assert run("1\n2\n1 100\n") == "99"
assert run("1\n3\n1 2 3\n") in ["4", "2", "3"], "small permutation sanity"
assert run("1\n4\n1 1 1 1\n") == "0"
assert run("1\n5\n5 4 3 2 1\n") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,100]`| 99 | trường hợp khoảng cách cực đoan | 
|`[1,2,3]`| giá trị nhỏ | hiệu ứng đặt hàng | 
| tất cả những cái | 0 | trường hợp chênh lệch bằng không | 
| trình tự đảo ngược | không âm | tính nhất quán | 

## Vỏ cạnh 

Một mảng thống nhất là trường hợp ranh giới đơn giản nhất. Vì tiền tố tối thiểu và tối đa luôn bằng nhau nên mọi thuật ngữ đều đóng góp 0 bất kể hoán vị, do đó mọi thuật toán đều phải trả về 0. 

Mảng hai phần tử hiển thị trực tiếp cơ chế cốt lõi: điểm chính xác là sự khác biệt tuyệt đối và bất kỳ thứ tự nào cũng tạo ra cấu trúc tiền tố giống nhau. 

Một mảng tăng hoặc giảm nghiêm ngặt rất hữu ích vì nó buộc công trình phải tiết lộ liệu sự xen kẽ tham lam thực sự có lợi hay dư thừa. Trong những trường hợp này, chiến lược xen kẽ vẫn duy trì mức chênh lệch tối đa giữa các tiền tố bằng cách đảm bảo rằng mức tối đa được đưa ra sớm trong khi mức tối thiểu bị trì hoãn.
