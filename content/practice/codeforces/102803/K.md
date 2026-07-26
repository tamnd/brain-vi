---
title: "CF 102803K - Giữ bí mật"
description: "Chúng tôi đang đếm những cây có gốc được dán nhãn. Nút i đi kèm với hai thông tin: độ sâu cần thiết của nó tính từ gốc và vị trí mới nhất mà nó được phép xuất hiện trong thứ tự tìm kiếm đầu tiên theo chiều rộng của cây."
date: "2026-07-26T16:27:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "K"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 50
verified: true
draft: false
---

[CF 102803K - Giữ bí mật](https://codeforces.com/problemset/problem/102803/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm những cây có gốc được dán nhãn. nút`i`đi kèm với hai thông tin: độ sâu cần thiết của nó từ gốc và vị trí mới nhất nó được phép xuất hiện theo thứ tự tìm kiếm theo chiều rộng đầu tiên của cây. 

Thứ tự theo chiều rộng đầu tiên không chỉ là bất kỳ thứ tự nào của các nút có cùng độ sâu. Các nút được xử lý theo cấp độ và các nút con của nút xuất hiện dưới dạng một khối liên tiếp. Nếu một nút xuất hiện trước nút khác, nút cha của chúng phải tuân theo quy tắc sắp xếp tương tự. Đây chính xác là thứ tự BFS thông thường được tạo ra bằng cách chọn thứ tự cho con của mỗi nút. 

Đầu vào chỉ mô tả các hạn chế của mọi nhãn. Đầu ra là số lượng cây khác nhau có thứ tự BFS thỏa mãn mọi ràng buộc, modulo`10^9 + 7`. 

Với`n`đạt tới`100000`trong một thử nghiệm duy nhất và tổng kích thước đạt`510000`, bất kỳ giải pháp nào cố gắng xây dựng cây, liệt kê các đơn hàng BFS hoặc sử dụng lập trình động trên các cặp nút sẽ quá chậm. Chúng ta cần một giải pháp gần tuyến tính hoặc`O(n log n)`. Việc sắp xếp hạn chế các nút là chấp nhận được vì tổng số phần tử chỉ vài trăm nghìn. 

Có một số trường hợp việc triển khai bất cẩn có thể âm thầm thất bại. Nếu không có nút nào ở độ sâu`1`, không có root nào có thể. Ví dụ:```
2
2 2
2 2
```Câu trả lời đúng là`0`. Giải pháp chỉ kiểm tra thứ tự tương đối ở các cấp độ sâu hơn có thể vô tình đếm các cây không hợp lệ vì mỗi cây phải có chính xác một gốc. 

Nếu một nút bị ép vào một vị trí trước khi khối độ sâu của nó bắt đầu thì câu trả lời phải bằng 0. Ví dụ:```
2
1 1
2 1
```Nút thứ hai cần độ sâu`2`, nhưng vị trí BFS duy nhất nó có thể đảm nhận là vị trí gốc. Câu trả lời đúng là`0`. 

Một lỗi phổ biến khác là quên rằng các nút con tạo thành một khối liên tục theo thứ tự BFS. Các lựa chọn gốc không độc lập đối với mọi nút. Ví dụ:```
3
1 1
2 3
2 3
```Câu trả lời là`2`. Hai nút sâu có thể được sắp xếp theo hai cách, nhưng cả hai đều phải là con của nút gốc duy nhất. Đối xử với mỗi đứa trẻ như một sự lựa chọn độc lập của cha mẹ sẽ là quá đáng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp trước tiên sẽ tạo ra mọi thứ tự BFS có thể có của nhãn thỏa mãn các ràng buộc về độ sâu. Sau khi chọn thứ tự như vậy, chúng ta có thể quyết định cách chia từng cấp thành các khối con và lấy cây. Điều này đúng vì cây BFS được mô tả đầy đủ theo thứ tự các nút bên trong mỗi cấp độ sâu cùng với cách phân chia mỗi cấp giữa các cấp độ gốc trước đó. 

Vấn đề là số lượng đơn đặt hàng BFS có thể có. Ngay cả khi chỉ có một mức độ sâu chứa nhiều nút, số hoán vị vẫn là giai thừa. Vì`100000`các nút, việc liệt kê chúng sẽ yêu cầu nhiều hơn`100000!`hoạt động, điều đó là không thể. 

Quan sát chính là cấu trúc cây và hoán vị BFS có thể được tách rời. Số cách để xây dựng mối quan hệ cha mẹ chỉ phụ thuộc vào số lượng nút tồn tại ở mỗi độ sâu chứ không phụ thuộc vào nhãn nào chiếm các vị trí đó. 

Đối với một mức độ cố định, giả sử có`a`các nút ở độ sâu hiện tại và`b`các nút ở độ sâu tiếp theo. các`b`trẻ em trong lớp BFS tiếp theo phải được chia thành`a`các nhóm liên tiếp, một nhóm cho mỗi nút hiện tại. Các nhóm trống được cho phép vì một nút có thể không có nút con. Số lượng phân chia như vậy là số lượng các thành phần yếu của`b`vào trong`a`bộ phận:$$\binom{a+b-1}{a-1}$$Nhiệm vụ còn lại là đếm các phép gán nhãn hợp lệ cho các vị trí BFS. Các nút có cùng độ sâu chiếm một khoảng vị trí liên tục. Nếu các vị trí được phép của chúng được sắp xếp thì nút vị trí được phép nhỏ nhất sẽ được chỉ định trước tiên. Khi`j`-nút thứ theo thứ tự sắp xếp này được xử lý, nó có:$$\min(x_i, R)-L+1-(j-1)$$các vị trí sẵn có, ở đâu`[L, R]`là khoảng độ sâu của nó. Nhân các lựa chọn này sẽ cho số lượng hoán vị hợp lệ cho cấp độ đó. 

Hai phần độc lập được nhân với nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu nút tồn tại ở mọi độ sâu. Nếu độ sâu`1`không chứa chính xác một nút thì cây không thể tồn tại. Chúng tôi cũng từ chối các khoảng trống trong đó tồn tại một cấp độ sâu hơn mà không tồn tại tất cả các cấp độ trước đó, bởi vì mọi nút không phải gốc đều cần có nút gốc. 
2. Ở mỗi độ sâu, hãy thu thập`x_i`giá trị của các nút ở độ sâu đó và sắp xếp chúng. Các nút có một độ sâu chỉ có thể chiếm các vị trí BFS liên tiếp thuộc độ sâu đó, do đó mỗi độ sâu có thể được xử lý độc lập. 
3. Đối với độ sâu với khoảng vị trí`[L, R]`, xử lý các giới hạn được sắp xếp từ nhỏ nhất đến lớn nhất. Nút hiện tại phải được đặt ở đâu đó giữa`L`Và`min(R, x_i)`. Trừ các vị trí đã được gán cho các nút trước đó. Nếu số lượng lựa chọn trở thành 0 hoặc âm thì không tồn tại thứ tự BFS hợp lệ. 
4. Tính toán trước giai thừa và giai thừa nghịch đảo. Với mỗi cặp độ sâu liên tiếp, hãy nhân số lần phân chia khối con có thể có:$$\binom{cnt[d]+cnt[d+1]-1}{cnt[d]-1}$$Điều này giải thích cho tất cả các hình dạng cây có thể tương thích với thứ tự BFS. 

1. Nhân số thứ tự BFS và số lượng hình dạng cây theo modulo`10^9+7`. 

Tại sao nó hoạt động: mỗi cây hợp lệ tạo ra chính xác một thứ tự BFS. Đối với thứ tự đó, mọi cấp độ có thể được xem độc lập khi gán nhãn vì tất cả các nút có độ sâu đều chiếm một khoảng cố định. Sau khi các nhãn được cố định theo thứ tự BFS, quyền tự do duy nhất còn lại là cách mỗi cấp độ được phân chia giữa các cấp độ gốc từ cấp độ trước đó. Công thức sao và thanh đếm chính xác các phân vùng đó. Hai lựa chọn này độc lập nên việc nhân chúng sẽ tính mỗi cây hợp lệ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve_case(n, nodes):
    depths = {}
    xs = {}
    max_depth = 0

    for d, x in nodes:
        depths[d] = depths.get(d, 0) + 1
        xs.setdefault(d, []).append(x)
        max_depth = max(max_depth, d)

    if depths.get(1, 0) != 1:
        return 0

    for d in range(1, max_depth + 1):
        if d not in depths:
            return 0

    cnt = [0] * (max_depth + 2)
    for d, c in depths.items():
        cnt[d] = c

    fact = [1] * (n + 2)
    for i in range(1, n + 2):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (n + 2)
    invfact[-1] = pow(fact[-1], MOD - 2, MOD)
    for i in range(n + 1, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    ans = 1
    start = 1
    for d in range(1, max_depth + 1):
        arr = xs[d]
        arr.sort()
        end = start + len(arr) - 1
        used = 0
        for x in arr:
            choices = min(end, x) - start + 1 - used
            if choices <= 0:
                return 0
            ans = ans * choices % MOD
            used += 1
        start = end + 1

    for d in range(1, max_depth):
        ans = ans * comb(cnt[d] + cnt[d + 1] - 1, cnt[d] - 1) % MOD

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        nodes = [tuple(map(int, input().split())) for _ in range(n)]
        out.append(str(solve_case(n, nodes)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai nhóm các nút theo độ sâu và lưu trữ các vị trí BFS tối đa được phép của chúng. Số lượng độ sâu đủ để xác định số lượng cấu trúc cây có thể có sau này. 

Mảng giai thừa được tạo một lần cho mỗi trường hợp thử nghiệm vì đối số nhị thức lớn nhất nằm bên dưới`n`. Hàm kết hợp sau đó là thời gian không đổi bằng cách sử dụng nghịch đảo mô-đun. 

Việc tính toán thứ tự là phần tế nhị nhất. Biến`start`là vị trí BFS đầu tiên của độ sâu hiện tại và`end`là cái cuối cùng Việc sắp xếp các giới hạn là cần thiết vì việc gán các giới hạn chặt chẽ nhất trước tiên sẽ mang lại tính tham lam tiêu chuẩn cho các hoán vị hợp lệ. Biến`used`biểu thị có bao nhiêu vị trí trong khoảng thời gian đã được chiếm giữ. 

Vòng lặp nhân thứ hai đếm các nhóm cha mẹ có thể có giữa các độ sâu liền kề. Công thức sử dụng`cnt[d] - 1`vì thành phần yếu của`cnt[d+1]`trẻ em vào`cnt[d]`các nhóm có`cnt[d+1] + cnt[d] - 1`tổng số vị trí trong phép biến đổi sao và thanh. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
1
9
1 1
2 3
2 3
2 5
3 6
3 7
3 8
3 8
3 9
```Số lượng độ sâu là: 

| Độ sâu | Nút | Vị trí BFS | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 2 đến 4 | 
| 3 | 5 | 5 đến 9 | 

Đối với độ sâu hai: 

| Thứ tự nút theo x | Hiện tại x | Lựa chọn có sẵn | Sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 
| 2 | 3 | 1 | 2 | 
| 3 | 5 | 1 | 2 | 

Đối với độ sâu ba: 

| Thứ tự nút theo x | Hiện tại x | Lựa chọn có sẵn | Sản phẩm | 
| --- | --- | --- | --- | 
| 1 | 6 | 2 | 2 | 
| 2 | 7 | 2 | 4 | 
| 3 | 8 | 2 | 8 | 
| 4 | 8 | 1 | 8 | 
| 5 | 9 | 1 | 8 | 

Đóng góp đặt hàng BFS là`2 * 8 = 16`. 

Những đóng góp hình dạng là: 

| Cấp độ | Công thức | Giá trị | 
| --- | --- | --- | 
| 1 đến 2 | C(1+3-1,0) | 1 | 
| 2 đến 3 | C(3+5-1,2) | 21 | 

Câu trả lời là`16 * 21 = 336`. 

Một ví dụ tùy chỉnh nhỏ hơn:```
1
3
1 1
2 3
2 3
```Số lượng đặt hàng là`2`vì hai đứa có thể hoán đổi vị trí cho nhau. Sự đóng góp hình dạng là`1`, vì cả hai nút phải là con của nút gốc. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| Đếm độ sâu | [1, 2] | 
| Số lượng đặt hàng BFS | 2 | 
| Số lượng hình dạng | 1 | 
| Câu trả lời cuối cùng | 2 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp tất cả các nhóm chuyên sâu chiếm ưu thế trong công việc. | 
| Không gian | O(n) | Các nhóm độ sâu, mảng giai thừa và dữ liệu tạm thời đều lưu trữ thông tin tuyến tính. | 

Tổng số nút trên tất cả các trường hợp thử nghiệm được giới hạn bởi`510000`, do đó, việc sắp xếp từng nút một lần sẽ vừa vặn trong giới hạn. Thuật toán tránh mọi sự phụ thuộc vào số lượng cây có thể có hoặc hoán vị BFS. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

# Assume solve_case from the solution is imported here.

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    ans = []
    for _ in range(t):
        n = int(next(it))
        nodes = []
        for _ in range(n):
            nodes.append((int(next(it)), int(next(it))))
        ans.append(str(solve_case(n, nodes)))
    return "\n".join(ans)

assert run("""1
9
1 1
2 3
2 3
2 5
3 6
3 7
3 8
3 8
3 9
""") == "336", "sample"

assert run("""1
1
1 1
""") == "1", "single root"

assert run("""1
2
2 2
2 2
""") == "0", "missing root"

assert run("""1
3
1 1
2 3
2 3
""") == "2", "two children swap"

assert run("""1
4
1 1
2 2
2 4
3 4
""") == "1", "tight position bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu gốc | 336 | Đếm đầy đủ thứ tự và cấu trúc cây | 
| Một nút | 1 | Cây hợp lệ tối thiểu | 
| Hai nút sâu hai không có gốc | 0 | Cấu hình độ sâu không hợp lệ | 
| Root có hai con | 2 | BFS đặt hàng đa dạng | 
| Giới hạn vị trí chặt chẽ | 1 | Xử lý ranh giới trong vị trí tham lam | 

## Vỏ cạnh 

Đối với cây không có gốc, thuật toán ngay lập tức trả về 0 vì độ sâu phải chứa chính xác một nút. Trong đầu vào:```
2
2 2
2 2
```bộ đếm độ sâu phát hiện ra điều đó`cnt[1]`bằng 0 và dừng trước bất kỳ phép tính tổ hợp nào. 

Đối với một nút có vị trí được phép nằm ngoài khoảng độ sâu của nó, vị trí tham lam sẽ phát hiện lỗi. TRONG:```
2
1 1
2 1
```độ sâu hai khoảng chỉ là vị trí`2`, nhưng vị trí khả dụng duy nhất được nút cho phép là vị trí`1`. Số lượng lựa chọn trở thành 0 và câu trả lời bị từ chối. 

Đối với nhiều nút ở cùng cấp độ, thuật toán không bao giờ chọn nút cha một cách độc lập. TRONG:```
3
1 1
2 3
2 3
```cả hai nút sâu đều được đặt ở vị trí hai và ba theo hai thứ tự có thể. Công thức hình dạng đưa ra một khả năng nhóm vì chỉ có một cha mẹ. Số cuối cùng chính xác là hai, khớp với hai chuỗi BFS có thể có.
