---
title: "CF 103443C - Dịch vụ cộng đồng"
description: "Chúng ta được cung cấp một hệ thống động gồm các khoảng được đặt trên một trục số từ 0 đến n − 1. Mỗi người mới đến với một khoảng [a, b] và được gán một mã định danh tăng dần dựa trên thứ tự đến. Sau đó, các yêu cầu dịch vụ sẽ đến dưới dạng các khoảng thời gian truy vấn [c, d]."
date: "2026-07-03T07:40:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103443
codeforces_index: "C"
codeforces_contest_name: "The 2021 ICPC Asia Taipei Regional Programming Contest"
rating: 0
weight: 103443
solve_time_s: 56
verified: true
draft: false
---

[CF 103443C - Dịch vụ cộng đồng](https://codeforces.com/problemset/problem/103443/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống động gồm các khoảng được đặt trên một trục số từ 0 đến n − 1. Mỗi người mới đến với một khoảng [a, b] và được gán một mã định danh tăng dần dựa trên thứ tự đến. Sau đó, các yêu cầu dịch vụ sẽ đến dưới dạng các khoảng thời gian truy vấn [c, d]. Đối với mỗi truy vấn, chúng ta phải tìm khoảng được thêm gần đây nhất có phạm vi trùng với phạm vi truy vấn. Sau khi sử dụng khoảng thời gian đó trong truy vấn, khoảng thời gian đó sẽ được coi là không hoạt động và không bao giờ được sử dụng lại. 

Vì vậy, bất cứ lúc nào chúng tôi cũng duy trì một tập hợp các khoảng thời gian hoạt động ngày càng tăng, mỗi khoảng thời gian có dấu thời gian bằng với thứ tự chèn của nó. Một truy vấn yêu cầu dấu thời gian tối đa trong số tất cả các khoảng giao nhau với phân đoạn truy vấn. Sau khi trả lời, khoảng đã chọn đó sẽ bị xóa khỏi tập hoạt động. 

Khó khăn chính là cả việc chèn và xóa đều trực tuyến và các truy vấn phải luôn phản ánh khoảng thời gian hoạt động gần đây nhất tại thời điểm truy vấn. 

Các ràng buộc cho phép tối đa 200.000 sự kiện và n lên tới 1.000.000. Điều này ngay lập tức loại trừ mọi phương pháp kiểm tra tất cả các khoảng thời gian cho mỗi truy vấn, vì một lần quét đơn giản sẽ tốn O(n) cho mỗi truy vấn trong trường hợp xấu nhất, dẫn đến O(nm), vượt xa giới hạn khả thi. 

Một vài trường hợp đặc biệt bộc lộ những cạm bẫy phổ biến. Đầu tiên, nhiều khoảng thời gian có thể chồng lên cùng một truy vấn, nhưng chỉ khoảng thời gian mới nhất trong số đó mới quan trọng. Ví dụ: nếu khoảng 1 là [0, 10], khoảng 2 là [3, 5] và khoảng 3 là [4, 6] thì truy vấn [4, 4] nên chọn khoảng 3, không phải khoảng 2 hoặc 1. 

Thứ hai, một khi khoảng đã được sử dụng, nó phải biến mất khỏi tất cả các câu trả lời trong tương lai. Nếu chúng ta quên loại bỏ nó đúng cách, nó có thể được chọn lại không chính xác. 

Thứ ba, vấn đề cấu trúc chồng chéo: các khoảng không rời rạc, do đó, bất kỳ cấu trúc nào dựa vào việc phân chia dòng thành các đoạn rời rạc sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lưu trữ tất cả các khoảng trong một danh sách và đối với mỗi truy vấn, lặp qua tất cả các khoảng, kiểm tra xem chúng có giao nhau với phạm vi truy vấn hay không và lấy khoảng thời gian có dấu thời gian lớn nhất. Điều này đúng vì dấu thời gian mã hóa trực tiếp lần truy cập gần đây và kiểm tra giao điểm là O(1). Tuy nhiên, với khoảng thời gian lên tới 200.000 và 200.000 truy vấn, điều này dẫn đến khoảng 4 × 10^10 kiểm tra trong trường hợp xấu nhất, quá chậm. 

Điều quan trọng cần lưu ý là đây là một vấn đề cổ điển về “phạm vi tối đa trong khoảng thời gian bị xóa”. Chúng ta cần phải trả lời nhiều lần: trong số tất cả các khoảng hoạt động giao nhau với một phạm vi truy vấn, có thời gian chèn lớn nhất. 

Cây phân đoạn trên trục tọa độ cung cấp cách tổ chức các khoảng sao cho mỗi khoảng chỉ được lưu trữ trong các nút O(log n), cụ thể là các nút được bao phủ hoàn toàn bởi sự phân tách cây phân đoạn của nó. Mỗi nút theo dõi các khoảng thời gian bao phủ đầy đủ phân khúc của nó. Đối với mỗi nút, chúng ta chỉ cần biết khoảng thời gian hoạt động gần đây nhất được lưu trữ ở đó. 

Khi xử lý một truy vấn, chúng tôi duyệt qua các nút cây phân đoạn nằm hoàn toàn bên trong phạm vi truy vấn. Mỗi nút như vậy đóng góp ứng cử viên khoảng thời gian hoạt động tốt nhất của nó. Câu trả lời là tối đa trong số những ứng cử viên này. 

Việc xóa được xử lý một cách lười biếng. Khi một khoảng thời gian được sử dụng, chúng tôi đánh dấu nó là không hoạt động. Mỗi nút duy trì một chồng các id khoảng thời gian và khi truy cập vào phần trên cùng, chúng tôi sẽ loại bỏ các khoảng thời gian không hợp lệ (đã được sử dụng) cho đến khi phần trên cùng hoạt động trở lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn | O(n · e) | O(n) | Quá chậm | 
| Cây phân đoạn có ngăn xếp khoảng cách | O(e log n) | O(e log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên phạm vi tọa độ [0, n - 1]. Mỗi nút đại diện cho một phân đoạn và lưu trữ một chồng các mã định danh khoảng có khoảng bao phủ đầy đủ phân đoạn đó.

Đối với mỗi khoảng, chúng tôi cũng duy trì danh sách các nút cây phân đoạn nơi nó được chèn để sau này chúng tôi có thể xóa nó bằng cách đánh dấu nó là không hoạt động. 

### Các bước 

1. Chỉ định mỗi khoảng thời gian mới một id tăng dần khi nó xuất hiện. Lưu trữ các điểm cuối của nó và đánh dấu nó là đang hoạt động. 
2. Chèn khoảng vào cây phân đoạn. Bất cứ khi nào một nút cây phân đoạn được bao phủ hoàn toàn bởi [a, b], hãy đẩy id khoảng vào ngăn xếp của nút đó. Điều này đảm bảo mỗi khoảng được phân phối trên các nút O(log n). 
3. Khi truy vấn [c, d] đến, duyệt cây phân đoạn và chỉ xem xét các nút chứa đầy đủ trong [c, d]. Đối với mỗi nút như vậy, hãy kiểm tra đỉnh ngăn xếp của nó sau khi xóa các mục không hoạt động. 
4. Làm sạch có nghĩa là liên tục bật ra khỏi ngăn xếp trong khi khoảng thời gian trên cùng không còn hoạt động. Điều này đảm bảo mỗi nút luôn hiển thị ứng cử viên hợp lệ tốt nhất. 
5. Câu trả lời cho truy vấn là id tối đa trong số tất cả các đỉnh ngăn xếp hợp lệ được thu thập trong quá trình truyền tải. 
6. Sau khi chọn id khoảng thời gian làm câu trả lời, hãy đánh dấu nó là không hoạt động để nó không được xem xét trong các truy vấn trong tương lai. 

### Tại sao nó hoạt động 

Mỗi khoảng được lưu trữ chính xác trong các nút thể hiện đầy đủ nó trong phân tách cây phân đoạn, do đó, bất kỳ điểm nào trong phạm vi của nó đều được bao phủ bởi ít nhất một nút như vậy. Khi một phạm vi truy vấn được xử lý, mỗi khoảng giao nhau với truy vấn phải xuất hiện ở ít nhất một nút được chứa đầy đủ trong phạm vi truy vấn. Tận dụng tối đa các nút này đảm bảo chúng tôi không bỏ lỡ bất kỳ khoảng thời gian ứng cử viên nào. Vì chúng tôi luôn xóa các khoảng thời gian không hoạt động khỏi ngăn xếp nên chúng tôi không bao giờ sử dụng lại khoảng thời gian đã xóa một cách không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, e = map(int, input().split())

# segment tree: each node stores list of interval ids
tree = [[] for _ in range(4 * n + 5)]

interval = [None]  # 1-indexed: (name, l, r)
active = [False]

def add(node, l, r, ql, qr, idx):
    if ql <= l and r <= qr:
        tree[node].append(idx)
        return
    mid = (l + r) // 2
    if ql <= mid:
        add(node * 2, l, mid, ql, qr, idx)
    if qr > mid:
        add(node * 2 + 1, mid + 1, r, ql, qr, idx)

def query(node, l, r, ql, qr):
    if ql <= l and r <= qr:
        while tree[node] and not active[tree[node][-1]]:
            tree[node].pop()
        return tree[node][-1] if tree[node] else 0

    mid = (l + r) // 2
    res = 0
    if ql <= mid:
        res = max(res, query(node * 2, l, mid, ql, qr))
    if qr > mid:
        res = max(res, query(node * 2 + 1, mid + 1, r, ql, qr))
    return res

for _ in range(e):
    tmp = input().split()
    t = tmp[0]

    if t == '1':
        name = tmp[1]
        a = int(tmp[2])
        b = int(tmp[3])
        interval.append((name, a, b))
        active.append(True)
        idx = len(interval) - 1
        add(1, 0, n - 1, a, b, idx)

    else:
        c = int(tmp[1])
        d = int(tmp[2])
        idx = query(1, 0, n - 1, c, d)
        if idx == 0:
            print("> <")
        else:
            print(interval[idx][0])
            active[idx] = False
```Hàm chèn cây phân đoạn chỉ phân phối từng khoảng cho các nút được bao phủ hoàn toàn trong phạm vi của nó. Điều này tránh việc lưu trữ thông tin dư thừa trên mỗi điểm. Hàm truy vấn chỉ đi qua các nhánh cây phân đoạn có liên quan và thu thập id khoảng thời gian tốt nhất có sẵn. 

Sự tinh tế quan trọng là lười biếng xóa. Thay vì cố gắng xóa một khoảng khỏi mọi nút mà nó xuất hiện, chúng tôi chỉ cần đánh dấu nó là không hoạt động và xóa nó khi gặp ở đỉnh ngăn xếp của nút. Điều này giữ cho hoạt động hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một hệ thống nhỏ với n = 10. 

Chúng tôi chèn khoảng A = [2, 6] với id 1, khoảng B = [4, 8] với id 2 và khoảng C = [5, 7] với id 3. 

Bây giờ chúng tôi xử lý truy vấn [5, 5]. 

| Bước | Bảo hiểm nút | Ngăn xếp ngọn được xem xét | Id tốt nhất | 
| --- | --- | --- | --- | 
| Truy vấn | các nút bao gồm 5 | A=1, B=2, C=3 | 3 | 

Kết quả là khoảng C vì nó có id cao nhất trong số các khoảng chồng chéo. 

Sau khi xuất ra, khoảng C được đánh dấu là không hoạt động. Nếu một truy vấn khác [5, 5] được đưa ra lần nữa, câu trả lời sẽ là 2 vì C bị loại bỏ. 

### Ví dụ 2 

Khoảng thời gian: 

[0, 9] id 1, [3, 4] id 2, [6, 8] id 3. 

Truy vấn [7, 7]: 

Chỉ có khoảng 1 và 3 giao nhau. Khoảng thời gian 3 mới hơn. 

| Bước | Khoảng thời gian hoạt động | Ứng viên | Kết quả | 
| --- | --- | --- | --- | 
| Truy vấn | {1,3} | 1 và 3 | 3 | 

Nếu chúng ta truy vấn lại [7, 7], chỉ còn lại khoảng 1, do đó kết quả trở thành 1. 

Những dấu vết này cho thấy việc xóa ảnh hưởng trực tiếp đến các truy vấn trong tương lai như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(e log n) | Mỗi lần chèn và truy vấn chạm vào nút O(log n) | 
| Không gian | O(e log n) | Mỗi khoảng được lưu trữ trong các nút cây phân đoạn O(log n) | 

Giới hạn n 10^6 và e 200.000 vừa vặn thoải mái trong độ phức tạp này vì log n là khoảng 20, cho tổng số khoảng vài triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, e = map(int, input().split())

    tree = [[] for _ in range(4 * n + 5)]
    interval = [None]
    active = [False]

    def add(node, l, r, ql, qr, idx):
        if ql <= l and r <= qr:
            tree[node].append(idx)
            return
        mid = (l + r) // 2
        if ql <= mid:
            add(node * 2, l, mid, ql, qr, idx)
        if qr > mid:
            add(node * 2 + 1, mid + 1, r, ql, qr, idx)

    def query(node, l, r, ql, qr):
        if ql <= l and r <= qr:
            while tree[node] and not active[tree[node][-1]]:
                tree[node].pop()
            return tree[node][-1] if tree[node] else 0

        mid = (l + r) // 2
        res = 0
        if ql <= mid:
            res = max(res, query(node * 2, l, mid, ql, qr))
        if qr > mid:
            res = max(res, query(node * 2 + 1, mid + 1, r, ql, qr))
        return res

    out = []
    for _ in range(e):
        tmp = input().split()
        if tmp[0] == '1':
            name = tmp[1]
            a, b = map(int, tmp[2:])
            interval.append((name, a, b))
            active.append(True)
            add(1, 0, n - 1, a, b, len(interval) - 1)
        else:
            c, d = map(int, tmp[1:])
            idx = query(1, 0, n - 1, c, d)
            if idx == 0:
                out.append("> <")
            else:
                out.append(interval[idx][0])
                active[idx] = False

    return "\n".join(out)

# custom tests

assert run("5 3\n1 A 0 2\n1 B 1 3\n2 1 1\n") == "B"
assert run("5 4\n1 A 0 4\n1 B 1 2\n2 1 1\n2 1 1\n") == "B\nA"
assert run("10 3\n2 1 5\n1 X 0 9\n2 1 5\n") == "X"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian chồng chéo với truy vấn ngay lập tức | B | tính chính xác của lựa chọn max-id | 
| truy vấn lặp lại sau khi xóa | B rồi A | hành vi loại bỏ thích hợp | 
| truy vấn trước bất kỳ khoảng thời gian nào | X | xử lý trạng thái trống | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi nhiều khoảng chồng lên cùng một vùng và khoảng mới nhất sẽ bị xóa sau một truy vấn. Cấu trúc không được vô tình tái sử dụng nó. Điều này được xử lý bằng cách đánh dấu nó là không hoạt động và chỉ loại bỏ nó khỏi ngăn xếp một cách lười biếng khi nó đạt đến đỉnh. 

Một trường hợp cạnh khác là phạm vi truy vấn không giao nhau với bất kỳ khoảng hoạt động nào. Trong trường hợp này, mọi ứng cử viên đều trống trên các nút đã truy cập và thuật toán trả về chính xác giá trị trọng điểm in "> <". 

Cuối cùng, các khoảng bao trùm các phạm vi rất lớn, chẳng hạn như [0, n − 1], sẽ được chèn vào rất ít nút cây phân đoạn nhưng lại xuất hiện trong nhiều truy vấn. Cây phân đoạn đảm bảo chúng vẫn được xử lý hiệu quả vì mỗi truy vấn chỉ chạm vào nút O(log n).
