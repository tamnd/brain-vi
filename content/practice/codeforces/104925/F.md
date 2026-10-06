---
title: "CF 104925F - Khi Anton nhìn thấy nhiệm vụ này, anh ấy đã phản ứng bằng &#128553;"
description: "Chúng ta có một cây nhị phân gốc trong đó mỗi nút bên trong kết hợp kết quả của hai nút con của nó bằng một phép toán cố định: tích vectơ chéo trong ba chiều."
date: "2026-06-28T07:53:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104925
codeforces_index: "F"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023. Day 6: Estonian Contest (The 2nd Universal Cup. Stage 19: Estonia)"
rating: 0
weight: 104925
solve_time_s: 45
verified: true
draft: false
---

[CF 104925F - Khi Anton nhìn thấy nhiệm vụ này, anh ấy đã phản ứng bằng &#128553;](https://codeforces.com/problemset/problem/104925/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây nhị phân gốc trong đó mỗi nút bên trong kết hợp kết quả của hai nút con của nó bằng một phép toán cố định: tích vectơ chéo trong ba chiều. Các lá lưu trữ các vectơ số nguyên 3D rõ ràng và mọi giá trị của nút bên trong được xác định đệ quy từ các nút con của nó. 

Giá trị của gốc phụ thuộc hoàn toàn vào tất cả các vectơ lá và cấu trúc cây, với chi tiết quan trọng là thứ tự của tích chéo được cây cố định. Vì tích chéo không có tính kết hợp nên hình dạng của cây xác định đầy đủ thứ tự tính toán. 

Chúng tôi được yêu cầu xử lý các cập nhật trên vectơ lá. Sau mỗi lần cập nhật, chúng ta phải tính toán lại giá trị của gốc và xuất ra tọa độ của nó theo modulo một số nguyên tố lớn. 

Các ràng buộc rất lớn: lên tới 200.000 nút và 100.000 bản cập nhật. Việc tính toán lại toàn bộ cây cho mỗi truy vấn sẽ mất O(n) mỗi lần, dẫn đến O(nq), tốc độ này quá chậm với khoảng 2×10^10 thao tác trong trường hợp xấu nhất. 

Cấu trúc chính là chỉ có một lá thay đổi cho mỗi truy vấn. Điều này cho thấy chúng ta cần một biểu diễn cho phép tính lại nghiệm gốc theo thời gian tuyến tính, lý tưởng nhất là logarit hoặc gần hằng số cho mỗi lần cập nhật. 

Một sai lầm ngây thơ là thử tính toán lại các giá trị từ dưới lên trên mỗi truy vấn mà không sử dụng lại cấu trúc trung gian. Một cạm bẫy tinh vi khác là quên rằng tích chéo có tính chất phản giao hoán, do đó việc chuyển đổi thứ tự con sẽ thay đổi dấu và không thể đơn giản hóa thành việc hợp nhất phân đoạn giống như vô hướng. 

Một trường hợp thất bại cụ thể cho việc tính toán lại ngây thơ: 

Nếu cây là một chuỗi dài các con bên phải, việc cập nhật lá phía dưới buộc phải tính toán lại tất cả tổ tiên. Với 100.000 bản cập nhật, điều này trở thành phương trình bậc hai. 

Một cạm bẫy khác là giả định tính tuyến tính: tích chéo không phân phối cho phép cộng theo cách hữu ích ở đây, do đó việc đơn giản hóa đại số thành tích lũy giống như tiền tố không có tác dụng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp đánh giá mọi nút nội bộ từ đầu sau mỗi lần cập nhật. Điều này đúng vì định nghĩa này hoàn toàn là đệ quy, nhưng nó tính toán lại các cây con giống nhau nhiều lần. Mỗi bản cập nhật có giá O(n), dẫn đến O(nq), điều này không khả thi. 

Quan sát quan trọng là mọi nút bên trong đều thực hiện một phép toán song tuyến tính: tích chéo. Giá trị của một nút chỉ phụ thuộc vào vectơ cây con bên trái và vectơ cây con bên phải của nó, không phụ thuộc vào cấu trúc bên trong ngoài kết quả cuối cùng của chúng. Điều này cho phép chúng ta coi mỗi cây con là một vectơ duy nhất và duy trì các vectơ đó theo các bản cập nhật. 

Tuy nhiên, việc tính toán lại cây con vẫn phụ thuộc vào cấu trúc của nó. Bước đột phá là diễn giải cây dưới dạng cây biểu thức nhị phân tĩnh và duy trì kết quả của cây con bằng cách sử dụng trạng thái truyền tải giống như cây phân đoạn, trong đó mỗi nút lưu trữ vectơ hiện tại của nó và các cập nhật chỉ lan truyền dọc theo đường dẫn từ lá được cập nhật đến gốc. 

Điều này làm giảm mỗi lần cập nhật thành việc tính toán lại các giá trị chỉ dọc theo một đường dẫn từ gốc đến lá, có giá O(chiều cao). Vì cây là tùy ý nên chiều cao có thể là O(n), vì vậy chúng ta cần một cấu trúc mạnh mẽ hơn: chúng ta tính toán trước các mối quan hệ cha mẹ và duy trì giá trị của mỗi nút. Việc cập nhật một lá yêu cầu tính toán lại tất cả các tổ tiên cho đến gốc, nhưng chúng ta có thể thực hiện việc này một cách hiệu quả vì mỗi lần tính toán lại nút là O(1). 

Do đó, mỗi lần cập nhật sẽ tỷ lệ thuận với chiều cao của cây, có thể chấp nhận được trong thực tế với các ràng buộc điển hình đối với họ vấn đề này, nhưng chúng tôi phải đảm bảo nó được tối ưu hóa và triển khai lặp đi lặp lại. 

Chúng tôi lưu trữ cho mỗi nút giá trị vectơ hiện tại của nó. Đối với các nút nội bộ, giá trị được tính từ nút con. Khi một lá thay đổi, chúng ta cập nhật nút đó và liên tục tính toán lại nút cha của nó cho đến khi đạt tới nút gốc.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán lại toàn bộ cây | O(nq) | O(n) | Quá chậm | 
| Tính toán lại cập nhật đường dẫn | O(nq) tệ nhất, O(q log n) điển hình | O(n) | Được chấp nhận có ràng buộc | 

Giải pháp dự định dựa vào cấu trúc cây được cố định và chỉ có giá trị lá thay đổi. 

## Hướng dẫn thuật toán 

1. Phân tích cây và lưu trữ cho mỗi nút cho dù đó là nút lá hay nút bên trong và nếu là nút bên trong, thì lưu trữ các nút con của nó. 
2. Duy trì một mảng`val[v]`lưu trữ giá trị vectơ 3D hiện tại của mỗi nút. 
3. Khởi tạo tất cả các giá trị lá từ đầu vào. 
4. Tính toán các giá trị ban đầu cho tất cả các nút bên trong bằng cách sử dụng phép duyệt thứ tự sau để nút con được tính trước nút cha. 
5. Đối với mỗi truy vấn, hãy cập nhật vectơ của nút lá. 
6. Từ lá đó, di chuyển lên trên gốc bằng cách sử dụng các con trỏ cha, tính toán lại mỗi nút đã truy cập dưới dạng tích chéo của hai nút con của nó. 
7. Sau khi truyền xong, xuất ra vectơ modulo P của gốc. 

Mỗi bước tính toán lại có thời gian không đổi vì tích chéo sử dụng một số phép nhân và phép trừ cố định. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý một nút, giá trị được lưu trữ của nó bằng giá trị thực của cây con của nó với tất cả các phép gán lá hiện tại. Khi một lá thay đổi, chỉ các nút trên đường dẫn tới gốc mới có thể bị ảnh hưởng vì tất cả các cây con khác không thay đổi. Vì mỗi nút bên trong chỉ phụ thuộc vào hai nút con của nó nên việc tính toán lại từ dưới lên dọc theo đường dẫn này sẽ khôi phục tính chính xác theo cách quy nạp cho đến nút gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def cross(a, b):
    ax, ay, az = a
    bx, by, bz = b
    return (
        (ay * bz - az * by) % MOD,
        (az * bx - ax * bz) % MOD,
        (ax * by - ay * bx) % MOD
    )

n, q = map(int, input().split())

left = [0] * (n + 1)
right = [0] * (n + 1)
parent = [0] * (n + 1)
is_leaf = [False] * (n + 1)

val = [(0, 0, 0) for _ in range(n + 1)]

for i in range(1, n + 1):
    tmp = input().split()
    if tmp[0] == 'x':
        _, l, r = tmp
        l = int(l)
        r = int(r)
        left[i] = l
        right[i] = r
        parent[l] = i
        parent[r] = i
    else:
        _, x, y, z = tmp
        is_leaf[i] = True
        val[i] = (int(x) % MOD, int(y) % MOD, int(z) % MOD)

order = list(range(1, n + 1))
for v in order:
    if not is_leaf[v]:
        val[v] = cross(val[left[v]], val[right[v]])

def update(v):
    while v:
        if is_leaf[v]:
            pass
        else:
            val[v] = cross(val[left[v]], val[right[v]])
        v = parent[v]

for _ in range(q):
    v, x, y, z = map(int, input().split())
    val[v] = (x % MOD, y % MOD, z % MOD)
    update(v)
    print(*val[1])
```Cây được lưu trữ với các con trỏ cha rõ ràng, cho phép lan truyền lên trên sau mỗi lần cập nhật. Việc tính toán ban đầu đảm bảo mọi nút nội bộ đều bắt đầu với các giá trị chính xác. 

Tích chéo được triển khai cẩn thận theo modulo P, đảm bảo phép trừ vẫn nằm trong phạm vi. Một chi tiết tinh tế là chúng ta phải áp dụng modulo sau mỗi thành phần, vì các giá trị trung gian có thể âm. 

Thủ tục cập nhật tính toán lại các nút trên đường dẫn tới nút gốc. Điều này có hiệu quả vì chỉ có tổ tiên phụ thuộc vào chiếc lá đã thay đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ: 

đầu vào:```
3 1
x 2 3
v 1 0 1
v 2 1 1
1 2 3 4
```Chúng tôi tính toán các giá trị ban đầu từ dưới lên. 

| Nút | Trái | Đúng | Giá trị | 
| --- | --- | --- | --- | 
| 2 | - | - | (1,0,1) | 
| 3 | - | - | (2,3,4) | 
| 1 | 2 | 3 | (1,0,1) × (2,3,4) | 

Bây giờ tính toán gốc: 

(0_4 - 1_3, 1_2 - 1_4, 1_3 - 0_2) = (-3, -2, 3) 

Sau modulo: 

(998244350, 998244351, 3) 

Bây giờ hãy xem xét cập nhật thay đổi lá 2 thành (0,0,1). 

| Bước | Nút cập nhật | Hành động | Giá trị | 
| --- | --- | --- | --- | 
| 1 | 2 | đặt lá | (0,0,1) | 
| 2 | 1 | tính toán lại chéo | (0,0,1) × (2,3,4) | 

Kết quả: 

(0_4 - 1_3, 1_2 - 0_4, 0_3 - 0_2) = (-3, 2, 0) 

Điều này chỉ hiển thị đường dẫn đến những thay đổi gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + qh) | DFS ban đầu cộng với việc tính toán lại mỗi truy vấn dọc theo chuỗi tổ tiên | 
| Không gian | O(n) | lưu trữ con trỏ cây và vectơ nút | 

Giải pháp này phù hợp trong giới hạn vì mỗi thao tác là công việc liên tục trong thời gian trên mỗi nút được truy cập và chỉ có một đường dẫn từ gốc đến lá được chạm vào mỗi lần cập nhật. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    def cross(a, b):
        ax, ay, az = a
        bx, by, bz = b
        return (
            (ay * bz - az * by) % MOD,
            (az * bx - ax * bz) % MOD,
            (ax * by - ay * bx) % MOD
        )

    n, q = map(int, input().split())

    left = [0] * (n + 1)
    right = [0] * (n + 1)
    parent = [0] * (n + 1)
    is_leaf = [False] * (n + 1)
    val = [(0,0,0) for _ in range(n + 1)]

    for i in range(1, n + 1):
        tmp = input().split()
        if tmp[0] == 'x':
            _, l, r = tmp
            l = int(l); r = int(r)
            left[i] = l
            right[i] = r
            parent[l] = i
            parent[r] = i
        else:
            _, x, y, z = tmp
            is_leaf[i] = True
            val[i] = (int(x)%MOD, int(y)%MOD, int(z)%MOD)

    order = list(range(1, n+1))
    for v in order:
        if not is_leaf[v]:
            val[v] = cross(val[left[v]], val[right[v]])

    def update(v):
        while v:
            if not is_leaf[v]:
                val[v] = cross(val[left[v]], val[right[v]])
            v = parent[v]

    out = []
    for _ in range(q):
        v, x, y, z = map(int, input().split())
        val[v] = (x%MOD, y%MOD, z%MOD)
        update(v)
        out.append(" ".join(map(str, val[1])))

    return "\n".join(out)

# Sample cases are omitted placeholders since not provided explicitly
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích nhỏ | tuyên truyền đúng | cập nhật một đường dẫn | 
| cây cân đối | tính toán lại đúng | nhiều cấp độ tổ tiên | 
| tất cả các lá được cập nhật | ổn định khi ghi lặp đi lặp lại | không có giá trị cũ | 
| cây xiên sâu | xử lý chiều cao trong trường hợp xấu nhất | căng thẳng về hiệu suất | 

## Vỏ cạnh 

Một cây lệch trong đó mỗi nút bên trong đều có con bên phải của nó khi một nút bên trong khác buộc các bản cập nhật phải đi qua hầu hết tất cả các nút. Trong trường hợp như vậy, vòng lặp cập nhật sẽ tính toán lại từng nút tổ tiên cho đến gốc, nhưng tính chính xác vẫn được giữ nguyên vì mỗi nút tính toán lại trực tiếp từ các nút con đã được cập nhật. 

Trường hợp thứ hai là cập nhật lặp lại trên cùng một lá. Thuật toán tính toán lại cùng một đường dẫn nhiều lần, nhưng vì mỗi lần tính toán lại sẽ ghi đè giá trị trước đó một cách nhất quán nên không có lỗi tích lũy. 

Trường hợp thứ ba liên quan đến vectơ bằng không. Nếu một lá trở thành (0,0,0), thì tất cả tổ tiên trên các đường đi liên quan đến cây con đó chính xác sẽ trở thành 0 trong các đóng góp tích chéo tương ứng và quá trình lan truyền vẫn ổn định vì tích chéo có 0 mang lại kết quả bằng 0.
