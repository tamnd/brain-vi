---
title: "CF 102439D - Trình diễn ánh sáng"
description: "Có (n) bóng đèn, do đó cấu hình của câu lạc bộ là một vectơ nhị phân có chiều dài (n). Công tắc là một vectơ nhị phân khác. Nhấn một công tắc có nghĩa là XOR vectơ của nó với cấu hình hiện tại, bởi vì mọi bóng đèn có bit tương ứng là một sẽ được bật."
date: "2026-08-10T06:49:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "D"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 475
verified: true
draft: false
---

[CF 102439D - Màn trình diễn ánh sáng](https://codeforces.com/problemset/problem/102439/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 55 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có (n) bóng đèn, do đó cấu hình của câu lạc bộ là một vectơ nhị phân có chiều dài (n). Công tắc là một vectơ nhị phân khác. Nhấn một công tắc có nghĩa là XOR vectơ của nó với cấu hình hiện tại, bởi vì mọi bóng đèn có bit tương ứng là một sẽ được bật. 

Mỗi công tắc chỉ khả dụng trong một khoảng thời gian liên tục trong ngày. Vào mỗi ngày, Dima có thể sử dụng bất kỳ tập hợp con nào của bộ chuyển mạch có sẵn trong ngày hôm đó, bao gồm cả tập hợp con trống. Câu hỏi mỗi ngày là có thể tạo ra bao nhiêu cấu hình bóng đèn khác nhau. 

Quan sát đại số quan trọng là thứ tự nhấn các công tắc không quan trọng. Nếu các công tắc khả dụng vào một ngày cụ thể là vectơ (v_1,v_2,\ldots,v_k), thì mọi cấu hình có thể truy cập đều có dạng 

[ 
c_1v_1\oplus c_2v_2\oplus\cdots\oplus c_kv_k, 
] 

trong đó mọi (c_i) đều bằng 0 hoặc bằng một. Do đó, các cấu hình có thể tiếp cận chính xác là khoảng tuyến tính của các vectơ có sẵn trên trường GF(2). 

Nếu các vectơ có thứ hạng (r), thì khoảng của chúng chứa chính xác (2^r) các vectơ khác nhau. Vì vậy, bài toán ban đầu tương đương với việc tính thứ hạng của tập hợp các vectơ chuyển đổi hoạt động mỗi ngày. 

Các giới hạn buộc chúng ta không thể xây dựng lại cơ sở Gaussian một cách độc lập hàng ngày. Chúng ta có thể có (d=q=500.000) và thậm chí một lần vượt qua tất cả các lần chuyển đổi mỗi ngày cũng sẽ yêu cầu (2,5\cdot10^{11}) bài kiểm tra chuyển đổi. Cách tiếp cận bậc hai hoặc (O(nq d)) là hoàn toàn không thể. 

Ngoài ra còn có một ràng buộc hữu ích ẩn trong định dạng đầu vào. Mỗi mô tả chuyển đổi chứa chính xác (n) bit và tổng độ dài của tất cả các chuỗi như vậy tối đa là (500.000). Do đó 

[ 
nq\le500.000. 
] 

Đây là điều làm cho giải pháp cơ sở Gaussian trở nên thực tế. Mặc dù bản thân (n) có thể là (500.000), trong trường hợp đó chỉ có thể có một công tắc. Ngược lại, nếu có (500.000) công tắc thì (n=1). 

Có một số trường hợp khó khăn mà việc triển khai đơn giản có thể xử lý sai. 

Hãy xem xét một công tắc số 0:```
1 1 1
1 1 0
```Công tắc duy nhất không thay đổi gì cả. Do đó, khoảng có hạng 0, vì vậy câu trả lời là`1`, không`2`. Việc triển khai bất cẩn bằng cách đếm các chuyển mạch thay vì các vectơ độc lập sẽ cho kết quả sai. 

Công tắc trùng lặp là một cái bẫy khác:```
2 2 1
1 1 10
1 1 10
```Cả hai công tắc đều có sẵn nhưng chúng biểu diễn cùng một vectơ. Thứ hạng là một, vì vậy câu trả lời là`2`. Việc coi mọi công tắc có sẵn như một lựa chọn nhị phân độc lập sẽ tạo ra sai sót`4`. 

Điểm cuối khoảng thời gian cũng phải được bao gồm. Ví dụ:```
2 2 2
1 2 10
2 2 01
```Vào ngày đầu tiên chỉ có vectơ đầu tiên, cho`2`cấu hình. Vào ngày thứ hai, cả hai vectơ độc lập đều có sẵn, cho`4`. Đầu ra đúng là`2 4`. Việc triển khai diễn giải điểm cuối phù hợp là độc quyền sẽ mất nút chuyển thứ hai vào ngày thứ hai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý mỗi ngày một cách độc lập. Đối với một ngày cố định, thu thập mọi chuyển đổi có khoảng chứa ngày đó và chèn vectơ của nó vào cơ sở Gaussian trên GF(2). Nếu cơ sở kết quả có thứ hạng (r), đầu ra (2^r). 

Điều này đúng vì việc loại bỏ Gaussian cho chúng ta biết chính xác có bao nhiêu hướng độc lập mà các công tắc hiện có cung cấp. Tuy nhiên, trong trường hợp xấu nhất có thể có (500.000) ngày và (500.000) lần chuyển mạch. Việc xử lý mỗi công tắc mỗi ngày có thể yêu cầu (250.000.000.000) kiểm tra công tắc trước khi xem xét chi phí loại bỏ. 

Cấu trúc giúp chúng ta tiết kiệm là mọi công tắc đều hoạt động trong một khoảng thời gian. Thay vì hỏi công tắc nào đang hoạt động trong một ngày, chúng ta có thể lưu trữ một công tắc trên cây phân đoạn bao gồm chính xác những ngày công tắc đó hoạt động. 

Một khoảng có thể được biểu thị bằng các nút cây phân đoạn (O(\log d)). Trong quá trình duyệt cây theo chiều sâu, mọi switch được lưu trữ tại một nút đều hoạt động đối với mọi lá bên dưới nút đó. Chúng ta có thể chèn tất cả các vectơ như vậy vào một cơ sở tuyến tính, truy cập đệ quy các vectơ con và sau đó hoàn tác các phần chèn khi trở về từ nút. 

Việc hoàn tác việc loại bỏ Gaussian nghe có vẻ khó khăn vì việc loại bỏ thông thường sẽ phá hủy thông tin. Giải pháp là sử dụng cơ sở trong đó mỗi lần chèn thành công sẽ tạo ra chính xác một trục mới. Chúng tôi nhớ trục nào đã được tạo và khi khôi phục chỉ cần xóa trục đó. Bởi vì quá trình xử lý đệ quy quay trở lại theo thứ tự chèn ngược, nên không vectơ sau này có thể phụ thuộc vào mục cơ sở đã bị xóa. 

Việc biểu diễn một vectơ nhị phân dưới dạng một số nguyên Python làm cho việc triển khai trở nên đặc biệt thuận tiện. Bitwise XOR thực hiện phép cộng vectơ trên GF(2) và`bit_length()`cung cấp bit được đặt cao nhất, đó là trục được cơ sở sử dụng. 

Cây phân đoạn xử lý các khoảng thời gian, trong khi cơ sở khôi phục xử lý sự phụ thuộc tuyến tính giữa các bộ chuyển mạch. Hai phần này khớp với nhau vì mỗi nút đệ quy biểu thị một phạm vi ngày mà tất cả các vectơ được lưu trữ tại nút đó đều có sẵn đồng thời. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(ndq)) trong cách triển khai đơn giản | (O(nq)) | Quá chậm | 
| Tối ưu | (O(nq\log d)) trường hợp xấu nhất | (O(q\log d+n)) | Đã chấp nhận | 

Giới hạn (nq\le500.000) tạo ra thời gian tối ưu khoảng (O(500.000\log 500.000)), với hằng số thực tế tùy thuộc vào mức độ giảm cơ sở Gaussian là cần thiết. 

## Hướng dẫn thuật toán 

1. Đọc từng switch và chuyển chuỗi nhị phân của nó thành số nguyên. Bit (i) thể hiện bóng đèn (i) có được bật hay không. Biểu diễn số nguyên cho phép chúng ta thực hiện phép cộng vectơ bằng XOR thông thường. 
2. Xây dựng cây phân đoạn ngầm định theo ngày từ (1) đến (d). Đối với một công tắc có sẵn trên ([l,r]), hãy phân tách khoảng đó thành các nút cây phân đoạn chuẩn có phạm vi bằng nhau chính xác ([l,r]). Lưu trữ vectơ chuyển đổi trong mỗi nút như vậy. 
3. Duy trì cơ sở tuyến tính được lập chỉ mục theo bit đặt cao nhất của mỗi vectơ cơ sở. Để chèn một vectơ (x), hãy nhìn liên tục vào bit được đặt cao nhất của nó. Nếu trục tương ứng đã tồn tại, XOR vectơ cơ sở hiện có từ (x). Nếu trục trống, hãy đặt (x) ở đó và tăng thứ hạng lên một. 
4. Bất cứ khi nào thao tác chèn tạo ra một trục mới, hãy ghi lại trục đó vào ngăn xếp khôi phục. Nếu vectơ giảm về 0 thì nó phụ thuộc tuyến tính vào cơ sở hiện tại, do đó không cần ghi lại gì. 
5. Duyệt cây đoạn theo cách đệ quy. Tại một nút, hãy nhớ kích thước ngăn xếp khôi phục hiện tại, sau đó chèn mọi vectơ được lưu trữ tại nút đó. Tất cả các vectơ này đều hoạt động trong toàn bộ phạm vi nút, vì vậy chúng thuộc về cơ sở cho mỗi ngày con cháu. 
6. Nếu nút là lá biểu thị ngày (i), cơ sở hiện tại chứa chính xác các công tắc độc lập có sẵn vào ngày đó. Nếu thứ hạng của nó là (r), hãy viết (2^r\bmod(10^9+7)) làm câu trả lời cho ngày (i). 
7. Đối với nút bên trong, xử lý đệ quy cả hai nút con. Cơ sở được chia sẻ giữa hai đứa trẻ, nhưng mỗi đứa trẻ nhận được chính xác các vectơ đang hoạt động trong phạm vi của nó cộng với các vectơ được thừa hưởng từ tổ tiên của nó. 
8. Sau khi xử lý một nút, liên tục bật ngăn xếp khôi phục cho đến khi đạt đến điểm kiểm tra đã lưu. Đối với mỗi trục bị loại bỏ, hãy xóa mục nhập cơ sở đó và giảm thứ hạng. Điều này khôi phục cơ sở về chính xác trạng thái mà nó có khi vào nút. 

### Tại sao nó hoạt động 

Tại mỗi nút cây phân đoạn, điểm bất biến là cơ sở tuyến tính chứa chính xác các chuyển mạch có các khoảng bao phủ hoàn toàn đường đi từ gốc đến nút đó. Khi đến một lá, đường dẫn đó tương ứng với một ngày, do đó cơ sở chứa chính xác tất cả các công tắc hoạt động vào ngày đó. Việc loại bỏ Gaussian bảo toàn khoảng cách trong khi loại bỏ các vectơ phụ thuộc, nghĩa là thứ hạng của nó chính xác là thứ nguyên của không gian cấu hình có thể truy cập. Không gian vectơ có chiều (r) trên GF(2) chứa chính xác (2^r) vectơ, do đó giá trị được tạo ra ở mỗi lá chính xác là số lượng cấu hình ánh sáng có thể tiếp cận. 

Việc khôi phục là đúng vì các thao tác chèn được hoàn tác theo thứ tự ngược lại. Việc chèn thành công sẽ chiếm một trục trống trước đó và mọi lần chèn được thực hiện sau nó sẽ bị xóa trước tiên. Việc xóa trục đã ghi sẽ khôi phục cơ sở trước đó mà không ảnh hưởng đến vectơ cơ sở cũ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, q, d = map(int, input().split())

    # Use a power-of-two segment tree.
    size = 1
    while size < d:
        size <<= 1

    # events[v] contains vector integers assigned to segment-tree node v.
    # None avoids creating millions of empty Python lists.
    events = [None] * (2 * size)

    for _ in range(q):
        l, r, s = input().split()
        l = int(l) - 1
        r = int(r)

        # One object is shared by all segment-tree copies of this switch.
        vec = int(s, 2)

        l += size
        r += size

        while l < r:
            if l & 1:
                if events[l] is None:
                    events[l] = [vec]
                else:
                    events[l].append(vec)
                l += 1

            if r & 1:
                r -= 1
                if events[r] is None:
                    events[r] = [vec]
                else:
                    events[r].append(vec)

            l >>= 1
            r >>= 1

    # basis[p] is the vector whose highest set bit is p.
    basis = [0] * n
    changes = []
    rank = 0

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = [0] * d

    def add_vector(x):
        nonlocal rank

        while x:
            p = x.bit_length() - 1
            b = basis[p]

            if b:
                x ^= b
            else:
                basis[p] = x
                changes.append(p)
                rank += 1
                return

    def rollback(checkpoint):
        nonlocal rank

        while len(changes) > checkpoint:
            p = changes.pop()
            basis[p] = 0
            rank -= 1

    sys.setrecursionlimit(1_000_000)

    def dfs(v, left, right):
        checkpoint = len(changes)

        ev = events[v]
        if ev is not None:
            for x in ev:
                add_vector(x)

        if right - left == 1:
            if left < d:
                ans[left] = pow2[rank]
        else:
            mid = (left + right) >> 1
            dfs(v << 1, left, mid)
            dfs(v << 1 | 1, mid, right)

        rollback(checkpoint)

    dfs(1, 0, size)

    sys.stdout.write(" ".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Chuyển đổi đầu vào`int(s, 2)`là thủ thuật biểu diễn trung tâm. Ký tự ngoài cùng bên trái trở thành bit cao, nhưng hướng chính xác không quan trọng vì XOR và sự phụ thuộc tuyến tính không bị ảnh hưởng bởi việc đổi tên tọa độ một cách nhất quán. 

Việc chuyển đổi khoảng thời gian sử dụng phạm vi nửa mở`[l, r)`. Khoảng đầu vào`[l,r]`đầu tiên được thay đổi thành tọa độ dựa trên 0 bằng cách trừ đi một từ`l`, trong khi điểm cuối bên phải không thay đổi. Do đó, một công tắc hoạt động vào những ngày đầu vào`1..3`trở thành phạm vi nửa mở`[0,3)`. Đây là một cách thuận tiện để tránh các lỗi điểm cuối trong quá trình phân rã cây phân đoạn lặp lại. 

các`events`mảng có kích thước`2 * size`, Ở đâu`size`ít nhất là lũy thừa nhỏ nhất của hai`d`. Các nút trống được đại diện bởi`None`. Điều này quan trọng trong Python vì việc tạo ra hàng triệu danh sách trống sẽ tiêu tốn nhiều bộ nhớ hơn mức cần thiết. 

Cơ sở lưu trữ một vectơ cho mỗi bit xoay có thể. Khi một vectơ có bit cao nhất`p`, hiện có`basis[p]`có thể loại bỏ bit đó. Cuối cùng, vectơ trở thành 0, chứng tỏ sự phụ thuộc hoặc nó đạt đến một trục không được sử dụng và trở thành một vectơ cơ sở độc lập mới. 

Ngăn xếp khôi phục chỉ chứa các chỉ số trục chứ không phải bản sao của toàn bộ cơ sở. Giả sử việc chèn tạo ra trục`p`. Vào lúc đó`basis[p]`là số không. Mỗi lần chèn sau sẽ được hoàn tác trước lần chèn này, vì vậy việc xóa`basis[p]`khôi phục lại chính xác trạng thái trước đó. 

Câu trả lời được tính toán trước là`pow2[r]`. Thứ hạng không bao giờ vượt quá`n`, vậy chỉ`n+1`quyền lực là cần thiết. Số nguyên Python không bị tràn, nhưng việc giảm modulo lũy thừa (10^9+7) sẽ giữ cho các câu trả lời được lưu trữ ở mức nhỏ. 

Việc duyệt sử dụng cây lũy thừa hoàn chỉnh ngay cả khi`d`bản thân nó không phải là sức mạnh của hai. Lá có chỉ số ít nhất`d`bị bỏ qua. Việc phân tách khoảng chỉ chèn ngày thực, vì vậy những lá nhân tạo này không bao giờ ảnh hưởng đến câu trả lời hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3 3 3
1 3 011
3 3 101
3 3 001
```Các vectơ là`011`,`101`, Và`001`. 

| Ngày | Vectơ hoạt động | Trục độc lập | Xếp hạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`011`|`011`| 1 | 2 | 
| 2 |`011`|`011`| 1 | 2 | 
| 3 |`011`,`101`,`001`| ba vectơ độc lập | 3 | 8 | 

Vào ngày thứ nhất và thứ hai, chỉ có một hướng khác 0 khả dụng, do đó, có hai cấu hình có thể truy cập: sử dụng công tắc hoặc không sử dụng công tắc. 

Vào ngày thứ ba, ba vectơ độc lập. Đặc biệt, không cái nào trong số chúng có thể được tạo ra dưới dạng XOR của hai cái còn lại, vì vậy nhịp có ba chiều và chứa tất cả (2^3=8) cấu hình bóng đèn có thể có. 

### Mẫu 2 

Đầu vào là:```
4 3 4
2 4 1010
2 4 0101
3 4 1101
```Hai công tắc đầu tiên sẽ có sẵn vào ngày thứ hai, trong khi công tắc thứ ba bắt đầu vào ngày thứ ba. 

| Ngày | Vectơ hoạt động | Xếp hạng | Trả lời | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 1 | 
| 2 |`1010`,`0101`| 2 | 4 | 
| 3 |`1010`,`0101`,`1101`| 3 | 8 | 
| 4 |`1010`,`0101`,`1101`| 3 | 8 | 

Ngày đầu tiên không có công tắc, nhưng Dima có thể sử dụng tập hợp con trống, do đó có thể truy cập được cấu hình tắt hoàn toàn. Điều đó mang lại (2^0=1). 

Vào ngày thứ hai, hai vectơ độc lập. Vào ngày thứ ba, vectơ thứ ba thêm một hướng độc lập khác, tăng thứ hạng từ hai lên ba. Ba công tắc tương tự vẫn có sẵn vào ngày thứ tư, vì vậy câu trả lời vẫn là tám. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nq\log d)) trường hợp xấu nhất | Mỗi công tắc được lưu trữ trong các nút cây phân đoạn (O(\log d)) và mỗi lần chèn cơ sở thực hiện tối đa (O(n)) việc loại bỏ trục | 
| Không gian | (O(q\log d+n+d)) | Bản sao khoảng thời gian của cây phân đoạn, cơ sở, ngăn xếp khôi phục, câu trả lời và siêu dữ liệu cây | 

Ràng buộc đặc biệt mà tổng độ dài của tất cả các chuỗi chuyển đổi tối đa là (500.000) đưa ra (nq\le500.000). Do đó, công việc loại bỏ trường hợp xấu nhất bị giới hạn bởi khoảng (500.000\log 500.000), thay vì (nq d). Cơ sở khôi phục chứa tối đa (n) vectơ tại bất kỳ thời điểm nào và độ sâu đệ quy chỉ là (O(\log d)). 

Giải pháp cuộc thi ban đầu được thiết kế cho giới hạn (2)-giây, (256)-MB và các triển khai C++ được chấp nhận sử dụng cùng một sự kết hợp thiết yếu giữa phân tách khoảng thời gian và bảo trì cơ sở tuyến tính. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`với nó`solve()`chức năng lộ ra. Các trường hợp có kích thước tối đa được tạo ra thay vì viết ra theo nghĩa đen, giúp giữ cho tệp thử nghiệm có thể sử dụng được.```python
# test_solution.py
import io
import sys

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """3 3 3
1 3 011
3 3 101
3 3 001
"""
) == "2 2 8"

# Provided sample 2
assert run(
    """4 3 4
2 4 1010
2 4 0101
3 4 1101
"""
) == "1 4 8 8"

# Provided sample 3
assert run(
    """5 2 2
1 2 01101
1 1 10101
"""
) == "4 2"

# Minimum-size case.
assert run(
    """1 1 1
1 1 0
"""
) == "1"

# Duplicate vectors: rank is one, not two.
assert run(
    """2 2 1
1 1 10
1 1 10
"""
) == "2"

# Endpoint test: the second switch exists only on day 2.
assert run(
    """2 2 2
1 2 10
2 2 01
"""
) == "2 4"

# Zero vector plus a nonzero vector.
assert run(
    """2 3 3
1 3 00
2 3 10
3 3 01
"""
) == "1 2 4"

# Maximum q and d with n = 1.
# Every switch is the same vector, so the rank is always one.
q = 500_000
d = 500_000

maximum_input = (
    f"1 {q} {d}\n"
    + ("1 " + str(d) + " 1\n") * q
)

maximum_expected = " ".join(["2"] * d)

assert run(maximum_input) == maximum_expected

# Maximum n with q = 1.
# The single vector is active for every day, so every answer is 2.
n = 500_000
d = 5
large_vector = "1" * n

large_input = f"{n} 1 {d}\n1 {d} {large_vector}\n"

assert run(large_input) == "2 2 2 2 2"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 1 0`|`1`| Kích thước tối thiểu và vector bằng 0 | 
| Hai giống hệt nhau`10`vectơ |`2`| Công tắc trùng lặp phụ thuộc | 
|`10`vào ngày 1 đến ngày 2 và`01`vào ngày thứ 2 |`2 4`| Điểm cuối khoảng bao gồm | 
|`00`,`10`,`01`với các khoảng so le |`1 2 4`| Không có vectơ và tăng thứ hạng | 
| (n=1,q=d=500000) | 500000 bản sao`2`| Số lượng công tắc và ngày tối đa | 
| (n=500000,q=1) | năm bản sao của`2`| Độ dài vectơ tối đa và ràng buộc tổng chuỗi | 

## Vỏ cạnh 

Một vectơ 0 được xử lý một cách tự nhiên bởi cơ sở. Vì```
1 1 1
1 1 0
```biểu diễn số nguyên bằng 0, vì vậy`add_vector(0)`ngay lập tức trả về mà không tạo trục. Thứ hạng vẫn bằng 0 và lá nhận được`2^0 = 1`. Công tắc tồn tại nhưng việc nhấn vào nó không thể thay đổi cấu hình đèn. 

Các vectơ trùng lặp cũng được xử lý bằng cách loại bỏ Gaussian thay vì bằng cách đếm các chuyển mạch. Vì```
2 2 1
1 1 10
1 1 10
```cái đầu tiên`10`tạo ra một trục. thứ hai`10`được XOR với vectơ cơ sở hiện có và trở thành 0. Chỉ còn lại một trục, vì vậy câu trả lời là`2`. 

Điểm cuối bên phải bao gồm được xử lý bằng cách chuyển đổi`[l,r]`vào phạm vi nửa mở dựa trên 0`[l-1,r)`. Vì```
2 2 2
1 2 10
2 2 01
```công tắc đầu tiên chiếm cả hai lá, trong khi công tắc thứ hai chỉ chiếm lá thứ hai. Do đó, lá đầu tiên có hạng một và tạo ra`2`, trong khi lá thứ hai có hai vectơ độc lập và tạo ra`4`. 

Một ngày không có công tắc cũng hợp lệ. Tập hợp con trống của các công tắc luôn tồn tại, do đó, có thể truy cập được cấu hình tắt hoàn toàn ngay cả khi tập hoạt động trống. Cơ sở khi đó có hạng 0 và câu trả lời chính xác là một. 

Cuối cùng, vectơ có thể dài hơn nhiều so với một từ máy. Các số nguyên có độ chính xác tùy ý của Python rất hữu ích ở đây vì vectơ (500.000) bit được biểu diễn trực tiếp và XOR hoạt động trên tất cả các từ máy bên trong. Việc đảm bảo đầu vào ngăn trường hợp vectơ lớn này trùng với một số lượng lớn các công tắc, vì (nq\le500.000).
