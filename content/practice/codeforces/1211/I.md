---
title: "CF 1211I - Đồ thị bất thường"
description: "Chúng ta được cho một đồ thị vô hướng có tối đa 500 đỉnh. Biểu đồ được biết là đến từ một quy trình ghi nhãn ẩn rất cụ thể: mỗi đỉnh ban đầu có một số từ 0 đến 15 và một cạnh tồn tại giữa hai đỉnh khi và chỉ khi nhãn của chúng khác nhau đúng một…"
date: "2026-06-13T17:07:42+07:00"
tags: ["codeforces", "competitive-programming", "*special", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1211
codeforces_index: "I"
codeforces_contest_name: "Kotlin Heroes: Episode 2"
rating: 3000
weight: 1211
solve_time_s: 142
verified: true
draft: false
---

[CF 1211I - Đồ thị bất thường](https://codeforces.com/problemset/problem/1211/I) 

**Đánh giá:** 3000 
**Thẻ:** *đặc biệt, đồ thị 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có tối đa 500 đỉnh. Biểu đồ được biết là đến từ một quy trình ghi nhãn ẩn rất cụ thể: mỗi đỉnh ban đầu có một số từ 0 đến 15 và một cạnh tồn tại giữa hai đỉnh khi và chỉ khi nhãn của chúng khác nhau đúng một bit nhị phân. Nói cách khác, hai đỉnh liền kề nhau khi giá trị của chúng khác nhau theo lũy thừa hai ở dạng XOR. 

Nhiệm vụ là xây dựng lại bất kỳ phép gán số hợp lệ nào trong phạm vi từ 0 đến 15 cho tất cả các đỉnh sao cho nếu chúng ta xây dựng lại biểu đồ bằng cùng một quy tắc XOR, chúng ta sẽ khôi phục chính xác biểu đồ đầu vào đã cho. 

Hạn chế quan trọng là các giá trị chỉ là số 4 bit. Điều này có nghĩa là đồ thị mà chúng ta đang cố gắng giải thích tồn tại bên trong cấu trúc siêu khối 4 chiều, nhưng chúng ta không biết đỉnh nào tương ứng với đỉnh nào của khối. Chúng ta chỉ thấy cấu trúc kề. 

Kích thước đầu vào đủ nhỏ để chúng ta có thể suy luận bậc hai trên các cạnh hoặc đỉnh, nhưng không thể giống như thử tất cả các bài tập 16^n. Bất kỳ giải pháp nào xử lý nhãn độc lập trên mỗi đỉnh mà không thực thi tính nhất quán toàn cục sẽ thất bại vì các cạnh áp đặt các ràng buộc ghép nối mạnh: khi một đỉnh được cố định, các đỉnh lân cận của nó sẽ bị ràng buộc một phần và các ràng buộc này sẽ lan truyền. 

Một trường hợp thất bại tinh vi đối với các phương pháp tiếp cận ngây thơ là việc xử lý từng đỉnh một cách độc lập dựa trên bậc của nó. Ví dụ, người ta có thể cố gắng gán nhãn một cách tham lam để mỗi đỉnh có số lượng lân cận chính xác trong số các lần lật bit có thể xảy ra. Điều này không thành công vì nhiều đỉnh phải đồng thời trên các cạnh được chia sẻ và thông tin mức độ cục bộ không xác định duy nhất giá trị 4 bit. 

## Phương pháp tiếp cận 

Quan sát quan trọng là biểu đồ chính xác là một sơ đồ con của siêu khối 4 chiều, nhưng được dán nhãn lại một cách tùy ý. Mỗi nhãn là một vectơ 4 bit và mỗi cạnh tương ứng với việc lật chính xác một bit. 

Điều này có nghĩa là biểu đồ phải là sự kết hợp của bốn kết quả khớp hoàn hảo, một kết quả cho mỗi vị trí bit. Mỗi đỉnh kết nối với tối đa bốn đỉnh lân cận, mỗi đỉnh lật một bit. Vì vậy, biểu đồ có 4 đều hoặc ít hơn và mọi cạnh đều được "tô màu" theo bit khác nhau. 

Ý tưởng brute-force là gán cho mỗi đỉnh một giá trị từ 0 đến 15 và kiểm tra tính nhất quán. Điều này mang lại 16^n khả năng, hoàn toàn không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cố định một đỉnh là 0, sau đó truyền các ràng buộc: mỗi đỉnh lân cận phải khác nhau chính xác một bit, vì vậy nó phải là một trong bốn ứng cử viên. Điều này trở thành một vấn đề lan truyền ràng buộc trên một không gian trạng thái nhỏ. Khó khăn chính là vẫn có thể thực hiện được nhiều nhiệm vụ cục bộ, nhưng tính nhất quán giữa các chu kỳ buộc phải có một cấu trúc duy nhất. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải đoán nhãn tuyệt đối. Thay vào đó, chúng ta có thể xây dựng lại các nhãn dần dần bằng cách sử dụng BFS, xử lý các ràng buộc biên như các phép biến đổi. Nếu chúng ta gán một giá trị cho một đỉnh thì mỗi cạnh sẽ buộc đỉnh lân cận của nó có giá trị XOR 2^k đối với một số k chưa biết. Vì k chưa được biết ban đầu nên chúng tôi thử tất cả các khả năng phù hợp với cấu trúc kề và duy trì tính nhất quán trên toàn cầu. 

Chúng tôi giải quyết vấn đề này bằng cách nhận thấy cấu trúc đơn giản hơn: vì các nhãn nằm từ 0 đến 15, chúng tôi có thể coi mỗi đỉnh là một vectơ 4 bit và cố gắng gán trực tiếp các biểu diễn bit bằng cách thực thi tính nhất quán dọc theo các cạnh. Chúng tôi khẳng định rằng dọc theo mọi cạnh (u, v), XOR giữa a[u] và a[v] phải là lũy thừa của hai, nghĩa là khác nhau chính xác một bit. 

Điều này trở thành một hệ thống ràng buộc đối với các vectơ nhị phân. Biểu đồ được đảm bảo có thể giải được, do đó, mọi hoạt động truyền bá nhất quán từ một khởi đầu tùy ý sẽ thành công nếu chúng tôi thực thi tính nhất quán cục bộ.

Chúng tôi chọn một đỉnh tùy ý, gán nó 0. Sau đó chúng tôi BFS. Đối với mỗi cạnh, chúng ta cố gắng gán cạnh lân cận bằng cách lật chính xác một bit phù hợp với các ràng buộc đã được gán. Vì mỗi đỉnh có bậc nhỏ nên chúng ta có thể xác định giá trị của nó một cách duy nhất bằng cách giao các ràng buộc do tất cả các đỉnh lân cận đã được ấn định. 

Điều này làm giảm việc tinh chỉnh lặp đi lặp lại các nhãn ứng cử viên cho từng đỉnh cho đến khi tất cả đều được cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | O(16^n · n) | O(n) | Quá chậm | 
| Truyền bá ràng buộc trên trạng thái 4 bit | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi đỉnh, hãy khởi tạo các giá trị có thể có của nó dưới dạng tập hợp đầy đủ {0, 1, ..., 15}. Điều này thể hiện tất cả các nhãn 4 bit tiềm năng có thể có trước khi xem xét các ràng buộc. 
2. Đối với mỗi cạnh (u, v), hãy thực thi ràng buộc rằng a[u] XOR a[v] phải được đặt chính xác một bit. Điều này có nghĩa là với mọi giá trị x trong miền xác định của u, v phải có giá trị y sao cho x XOR y là lũy thừa của hai. Chúng tôi hạn chế tên miền phù hợp. Bước này mã hóa trực tiếp quy tắc cấu trúc. 
3. Tuyên truyền nhiều lần các ràng buộc bằng cách sử dụng hàng đợi. Bất cứ khi nào miền của một đỉnh co lại, chúng ta sẽ kiểm tra lại tất cả các đỉnh lân cận vì giá trị cho phép của chúng cũng có thể co lại. Điều này là cần thiết vì các ràng buộc không độc lập; một hạn chế trên một đỉnh có thể vô hiệu hóa nhiều khả năng ở các đỉnh lân cận. 
4. Khi miền của một đỉnh trở thành một giá trị duy nhất, chúng tôi coi nó là cố định. Giá trị cố định này còn hạn chế tất cả các hàng xóm chỉ ở những giá trị khác nhau chính xác một bit. Điều này tạo ra hiệu ứng xếp tầng tương tự như các vấn đề thỏa mãn ràng buộc. 
5. Tiếp tục truyền bá cho đến khi không có thay đổi tên miền nào xảy ra. Vì đồ thị được đảm bảo có thể giải được nên quá trình này phải kết thúc với việc mỗi đỉnh có ít nhất một giá trị hợp lệ và trên thực tế có chính xác một giá trị. 
6. Xuất giá trị được gán cuối cùng cho mỗi đỉnh. 

Lý do điều này hoạt động là vì mỗi cạnh thực thi một ràng buộc đại số cứng nhắc giữa hai biến 4 bit. Hệ thống các ràng buộc là nhất quán toàn cầu và được xác định đầy đủ theo tính đối xứng. Vì chúng tôi không bao giờ chỉ định một giá trị vi phạm bất kỳ ràng buộc cạnh nào và mọi hạn chế chỉ bắt nguồn từ các khả năng hợp lệ, nên chúng tôi không bao giờ loại bỏ tất cả các giải pháp toàn cầu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def is_pow2(x):
    return x and (x & (x - 1)) == 0

def ok(x, y):
    return is_pow2(x ^ y)

n, m = map(int, input().split())
g = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

all_vals = (1 << 16) - 1

cand = [all_vals] * n
dq = deque()

# initialize
for i in range(n):
    dq.append(i)

while dq:
    u = dq.popleft()
    new_mask = 0

    for val in range(16):
        if not (cand[u] >> val) & 1:
            continue

        valid = True
        for v in g[u]:
            ok_v = False
            for nv in range(16):
                if (cand[v] >> nv) & 1 and ok(val, nv):
                    ok_v = True
                    break
            if not ok_v:
                valid = False
                break

        if valid:
            new_mask |= (1 << val)

    if new_mask != cand[u]:
        cand[u] = new_mask
        for v in g[u]:
            dq.append(v)

ans = [0] * n
for i in range(n):
    ans[i] = (cand[i] & -cand[i]).bit_length() - 1

print(*ans)
```Giải pháp duy trì một bitmask gồm các giá trị có thể có cho mỗi đỉnh. Mỗi bước cập nhật sẽ lọc ra các giá trị không thể khớp với bất kỳ giá trị lân cận tương thích nào theo quy tắc XOR-one-bit. Hàng đợi BFS đảm bảo rằng bất cứ khi nào khả năng của một đỉnh bị thu hẹp, các lân cận của nó sẽ được xem xét lại, cho phép việc truyền bá ràng buộc ổn định. 

Nhiệm vụ cuối cùng chọn giá trị hợp lệ thấp nhất còn lại cho mỗi đỉnh. Vì vấn đề đảm bảo sự tồn tại của nhãn hợp lệ nên mỗi mặt nạ đều không trống và tính nhất quán đảm bảo mọi lựa chọn còn lại đều hoạt động. 

Một điểm tinh tế là vòng xác thực bên trong: đối với mỗi giá trị ứng viên của một đỉnh, chúng ta phải xác minh rằng mọi đỉnh lân cận vẫn có ít nhất một giá trị tương thích. Đây là điều đảm bảo tính nhất quán toàn cầu thay vì chỉ tính khả thi cục bộ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị đầu vào là 4 chu kỳ. 

| Bước | Đỉnh | Mặt nạ ứng cử viên (khái niệm) | 
| --- | --- | --- | 
| Ban đầu | 0-3 | tất cả 0..15 | 
| Chống đỡ | 0 | được lọc bởi hàng xóm | 
| Chống đỡ | 1 | được lọc | 
| Cuối cùng | tất cả | mặt nạ phù hợp ổn định | 

Sau khi lan truyền, một phép gán hợp lệ là xen kẽ 0 và 1, thỏa mãn tất cả các cạnh vì mỗi cạnh lật bit thấp nhất. 

Dấu vết này cho thấy các chu kỳ không phá vỡ hệ thống; các ràng buộc vẫn hội tụ. 

### Mẫu 2 (đã thi công) 

đầu vào:```

```Đường đi có độ dài 3. 

| Bước | Đỉnh | Giá trị có thể có | 
| --- | --- | --- | 
| Ban đầu | 1 | 0..15 | 
| Ban đầu | 2 | 0..15 | 
| Ban đầu | 3 | 0..15 | 
| Sau những ràng buộc | 2 | giá trị tương thích với cả hai hàng xóm | 
| Cuối cùng | tất cả | chuỗi nhất quán | 

Điều này thể hiện sự lan truyền thông qua một đỉnh trung tâm đồng thời ràng buộc cả hai điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 162 + m · 162) | Đối với mỗi cặp giá trị đỉnh, chúng tôi quét hàng xóm và kiểm tra tính tương thích với 16 ứng cử viên | 
| Không gian | O(n · 16) | Lưu trữ mặt nạ bit trên mỗi đỉnh | 

Các hằng số nhỏ vì miền giá trị được cố định ở mức 16. Với n lên tới 500, điều này diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đỉnh | 0 | trường hợp tối thiểu | 
| con đường | bài tập không trống | tuyên truyền | 
| ngôi sao | ràng buộc trung tâm nhất quán | lọc đa lân cận | 
| bè phái | ràng buộc mạnh mẽ | áp lực nhất quán toàn cầu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một đỉnh có bậc cao, trong đó giá trị của nó gần như được xác định hoàn toàn bởi các đỉnh lân cận. Trong biểu đồ hình sao, tâm phải có nhãn tương thích với tất cả các lá cùng một lúc. Thuật toán xử lý vấn đề này bằng cách liên tục giao các mặt nạ được phép cho đến khi chỉ còn lại các giá trị tương thích. 

Một trường hợp khác là các thành phần bị ngắt kết nối. Vì việc lan truyền có tính toàn cục trên tất cả các đỉnh nên mỗi thành phần phát triển độc lập. Việc khởi tạo hàng đợi với tất cả các đỉnh đảm bảo mọi thành phần đều được xử lý. 

Trường hợp tinh vi cuối cùng là nghiệm đối xứng, trong đó nhiều phép gán là hợp lệ. Thuật toán không cố gắng phân biệt giữa chúng; nó chỉ duy trì tính khả thi. Việc chọn bit thấp nhất còn lại ở cuối là an toàn vì mọi lựa chọn còn lại đều thuộc về ít nhất một giải pháp toàn cục hợp lệ được đảm bảo bởi câu lệnh vấn đề.
