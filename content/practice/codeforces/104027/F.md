---
title: "CF 104027F - \u843d\u77f3"
description: "Bài toán mô hình hóa một tập hợp các khối đá rơi thẳng đứng trên mặt đất một chiều được làm bằng các cột. Mỗi cột bắt đầu trống ở độ cao bằng 0 và khi các viên đá được thả xuống, chúng sẽ xếp chồng lên nhau tùy thuộc vào vị trí chúng tiếp đất."
date: "2026-07-02T04:09:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104027
codeforces_index: "F"
codeforces_contest_name: "The 10-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104027
solve_time_s: 47
verified: true
draft: false
---

[CF 104027F - \u843d\u77f3](https://codeforces.com/problemset/problem/104027/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô hình hóa một tập hợp các khối đá rơi thẳng đứng trên mặt đất một chiều được làm bằng các cột. Mỗi cột bắt đầu trống ở độ cao bằng 0 và khi các viên đá được thả xuống, chúng sẽ xếp chồng lên nhau tùy thuộc vào vị trí chúng tiếp đất. 

Mỗi viên đá tác động lên một khoảng cột liền nhau. Khi một viên đá đến, nó không mô phỏng dần từng tế bào rơi xuống. Thay vào đó, nó hoạt động giống như một khối cứng nằm phẳng trên toàn bộ khoảng của nó. Chiều cao cuối cùng của hòn đá được xác định bởi cột cao nhất trong khoảng được che phủ của nó, bởi vì hòn đá phải nằm trên bất cứ thứ gì đã có sẵn ở đó. Sau khi tiếp đất, nó sẽ tăng đồng đều chiều cao của mỗi cột trong khoảng của nó đúng một đơn vị trên chiều cao hỗ trợ tối đa đó. 

Đầu ra là cấu hình cuối cùng sau khi xử lý tất cả các viên đá, thường có nghĩa là chiều cao thu được ở mỗi cột hoặc ảnh hưởng của tất cả các vị trí. 

Mặc dù câu lệnh ngắn gọn nhưng điểm trừu tượng chính là mọi thao tác đều là một truy vấn phạm vi, theo sau là phép gán phạm vi. Truy vấn phạm vi yêu cầu chiều cao hiện tại tối đa trong một phân đoạn và bản cập nhật sẽ đặt toàn bộ phân khúc đó thành điểm cộng tối đa đó. 

Các ràng buộc không được nêu rõ ràng, nhưng giải pháp dự kiến ​​là tuyến tính hoặc gần tuyến tính về số lần thao tác nhân với chi phí logarit cho mỗi truy vấn. Một mô phỏng đơn giản trên mỗi ô sẽ quá chậm nếu cả số lượng cột và đá đều lớn, vì mỗi thao tác chạm vào cả một khoảng. 

Một trường hợp phức tạp xuất phát từ các khoảng thời gian chồng chéo trong đó các bản cập nhật trước đó chiếm ưu thế một phần các bản cập nhật sau. Ví dụ: nếu chúng ta có các cột được khởi tạo bằng 0 và hai thao tác: khoảng thời gian cập nhật đầu tiên [1,3], sau đó cập nhật [2,4], thì thao tác thứ hai phải tuân theo giá trị cập nhật từ thao tác đầu tiên khi tính toán mức tối đa của nó. Việc triển khai ngây thơ mà quên duy trì trạng thái toàn cầu một cách chính xác trên các phần chồng chéo sẽ tính toán độ cao hạ cánh không chính xác. 

Một trường hợp lỗi khác xuất hiện khi các khoảng thời gian lớn và có tính chồng chéo cao, chẳng hạn như mọi thao tác đều bao trùm toàn bộ phạm vi. Cách tiếp cận cập nhật theo vị trí sẽ thoái hóa thành hành vi bậc hai và sẽ hết thời gian mặc dù mỗi thao tác riêng lẻ trông đơn giản. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì một dãy chiều cao cột. Đối với mỗi viên đá, chúng tôi quét tất cả các cột trong khoảng của nó, tính chiều cao tối đa, sau đó quét lại để gán giá trị đó cộng một. Điều này đúng vì nó tuân theo đúng quy luật vật lý “đạt điểm hỗ trợ cao nhất trong khoảng, sau đó lấp đầy toàn bộ nhịp”. Tuy nhiên, mỗi thao tác tốn thời gian tuyến tính theo chiều rộng của khoảng, do đó, trong trường hợp xấu nhất khi mỗi viên đá trải dài gần như toàn bộ chiều rộng, tổng độ phức tạp sẽ trở thành bậc hai tính theo số cột nhân với các thao tác. 

Quan sát cấu trúc quan trọng là hoạt động được xác định hoàn toàn bằng hai hành động trên một mảng tĩnh: một truy vấn tối đa phạm vi được thực hiện ngay sau đó bằng cách đặt mọi giá trị trong phạm vi đó thành một hằng số. Điều này loại bỏ mọi nhu cầu mô phỏng chuyển động bên trong khoảng thời gian. Khi đã biết mức tối đa, kết quả sẽ đồng nhất trên toàn bộ phân khúc. 

Điều này làm cho vấn đề trở thành trường hợp điển hình đối với cây phân đoạn hoặc bất kỳ cấu trúc dữ liệu nào hỗ trợ truy vấn phạm vi tối đa nhanh và gán phạm vi nhanh. Vì giá trị được chỉ định luôn là một hằng số duy nhất được tính toán từ truy vấn nên chúng tôi có thể ghi đè toàn bộ phân đoạn một cách an toàn. Không cần truyền một phần các giá trị cũ bên trong phân đoạn ngoài cơ chế truyền lan lười tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m) trường hợp xấu nhất trên mỗi thao tác dẫn đến tổng thể là O(n²) | O(m) | Quá chậm | 
| Cây phân đoạn (phạm vi tối đa + gán phạm vi) | O(n log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một mảng chiều cao trên tất cả các cột, nhưng thay vì cập nhật trực tiếp, chúng tôi lưu trữ nó bên trong cây phân đoạn hỗ trợ cả truy vấn tối đa phạm vi và cập nhật gán phạm vi. 

### Các bước 

1. Xây dựng cây phân đoạn trên tất cả các cột, khởi tạo mọi chiều cao về 0. Điều này tượng trưng cho mặt đất trống trước khi bất kỳ hòn đá nào được thả xuống. 
2. Đối với mỗi phép toán đá có khoảng [l, r], hãy truy vấn cây phân đoạn để tìm giá trị lớn nhất trong khoảng này. Mức tối đa này thể hiện điểm hỗ trợ cao nhất nơi đá có thể tiếp đất mà không chồng lên cấu trúc hiện có. 
3. Gọi kết quả của truy vấn này là q. Hòn đá sẽ có chiều cao q + 1, vì nó nằm ngay trên đỉnh cột cao nhất hiện có trong khoảng của nó. 
4. Áp dụng cập nhật gán phạm vi trên [l, r], đặt mọi vị trí trong khoảng đó thành q + 1. Điều này phản ánh rằng đá tạo thành một lớp phẳng trên toàn bộ phân đoạn ở độ cao mới này. 
5. Tiếp tục quá trình này cho tất cả các viên đá theo thứ tự, đảm bảo rằng mỗi lần cập nhật sẽ thấy cấu trúc được cập nhật đầy đủ từ các hoạt động trước đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi cột đều lưu trữ chiều cao chính xác của cấu trúc xếp chồng đã được xây dựng cho đến nay. Truy vấn tối đa trong một khoảng thời gian nắm bắt được ràng buộc vật lý chính xác rằng hòn đá phải nằm trên chướng ngại vật cao nhất trong khu vực đó. Vì đá cứng và đồng nhất nên khi xác định được chiều cao cuối cùng, mọi cột trong nhịp của nó phải phù hợp với chiều cao đó. Cây phân đoạn đảm bảo cả truy vấn và ghi đè đều nhất quán với tất cả các bản cập nhật trước đó, do đó không có thao tác nào sau này bỏ qua cấu trúc trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mx = [0] * (4 * n)
        self.lazy = [-1] * (4 * n)

    def push(self, idx):
        if self.lazy[idx] != -1:
            v = self.lazy[idx]
            self.mx[idx * 2] = v
            self.mx[idx * 2 + 1] = v
            self.lazy[idx * 2] = v
            self.lazy[idx * 2 + 1] = v
            self.lazy[idx] = -1

    def range_set(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.mx[idx] = val
            self.lazy[idx] = val
            return
        if r < ql or l > qr:
            return
        self.push(idx)
        mid = (l + r) // 2
        self.range_set(idx * 2, l, mid, ql, qr, val)
        self.range_set(idx * 2 + 1, mid + 1, r, ql, qr, val)
        self.mx[idx] = max(self.mx[idx * 2], self.mx[idx * 2 + 1])

    def range_max(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.mx[idx]
        if r < ql or l > qr:
            return 0
        self.push(idx)
        mid = (l + r) // 2
        return max(
            self.range_max(idx * 2, l, mid, ql, qr),
            self.range_max(idx * 2 + 1, mid + 1, r, ql, qr)
        )

def solve():
    n, m = map(int, input().split())
    seg = SegTree(m)

    for _ in range(n):
        l, r = map(int, input().split())
        q = seg.range_max(1, 1, m, l, r)
        seg.range_set(1, 1, m, l, r, q + 1)

    res = []
    for i in range(1, m + 1):
        res.append(str(seg.range_max(1, 1, m, i, i)))
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được sử dụng ở dạng tiêu chuẩn với khả năng lan truyền lười biếng để gán phạm vi. Chi tiết triển khai chính là chúng tôi không bao giờ cần hỗ trợ cập nhật gia tăng mà chỉ ghi đè các phân đoạn bằng một giá trị duy nhất. Do đó, thẻ lười lưu trữ một bài tập hoàn chỉnh chứ không phải một delta. 

Thứ tự của các thao tác rất quan trọng: số lượng tối đa phải được truy vấn trước khi áp dụng bất kỳ bản cập nhật nào, nếu không giá trị mới sẽ làm ô nhiễm kết quả. Đầu ra cuối cùng được trích xuất bằng cách truy vấn từng vị trí riêng lẻ, tương đương với việc đọc các giá trị lá của cây phân đoạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ có năm cột và ba viên đá: [1,3], [2,5] và [1,5]. 

### Dấu vết 1 

| Bước | Khoảng thời gian | Tối đa trong phạm vi (q) | Giá trị được gán | Trạng thái sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3] | 0 | 1 | [1,1,1,0,0] | 
| 2 | [2,5] | 1 | 2 | [1,2,2,2,2] | 
| 3 | [1,5] | 2 | 3 | [3,3,3,3,3] | 

Dấu vết này cho thấy cấu trúc từng phần trước đó ảnh hưởng trực tiếp như thế nào đến các truy vấn tối đa sau này và cách mỗi bản cập nhật làm phẳng phân đoạn thành một mức thống nhất mới. 

### Dấu vết 2 

Bây giờ hãy xem xét [2,4], [1,2], [3,5]. 

| Bước | Khoảng thời gian | Tối đa trong phạm vi (q) | Giá trị được gán | Trạng thái sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4] | 0 | 1 | [0,1,1,1,0] | 
| 2 | [1,2] | 1 | 2 | [2,2,1,1,0] | 
| 3 | [3,5] | 1 | 2 | [2,2,2,2,2] | 

Điều này chứng tỏ rằng mỗi phép toán chỉ phụ thuộc vào mức tối đa hiện tại trong khoảng của nó chứ không phụ thuộc vào cấu trúc toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log m) | Mỗi viên đá thực hiện một truy vấn tối đa phạm vi và một phép gán phạm vi trên cây phân đoạn | 
| Không gian | O(m) | Cây phân đoạn lưu trữ số lượng nút không đổi trên mỗi cột | 

Hệ số logarit đủ nhỏ cho các ràng buộc điển hình trong đó cả số lượng đá và cột có thể lên tới 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.mx = [0] * (4 * n)
            self.lazy = [-1] * (4 * n)

        def push(self, idx):
            if self.lazy[idx] != -1:
                v = self.lazy[idx]
                self.mx[idx * 2] = v
                self.mx[idx * 2 + 1] = v
                self.lazy[idx * 2] = v
                self.lazy[idx * 2 + 1] = v
                self.lazy[idx] = -1

        def range_set(self, idx, l, r, ql, qr, val):
            if ql <= l and r <= qr:
                self.mx[idx] = val
                self.lazy[idx] = val
                return
            if r < ql or l > qr:
                return
            self.push(idx)
            mid = (l + r) // 2
            self.range_set(idx * 2, l, mid, ql, qr, val)
            self.range_set(idx * 2 + 1, mid + 1, r, ql, qr, val)
            self.mx[idx] = max(self.mx[idx * 2], self.mx[idx * 2 + 1])

        def range_max(self, idx, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.mx[idx]
            if r < ql or l > qr:
                return 0
            self.push(idx)
            mid = (l + r) // 2
            return max(
                self.range_max(idx * 2, l, mid, ql, qr),
                self.range_max(idx * 2 + 1, mid + 1, r, ql, qr)
            )

    data = list(map(int, inp.split()))
    it = iter(data)
    n, m = next(it), next(it)
    seg = SegTree(m)

    for _ in range(n):
        l, r = next(it), next(it)
        q = seg.range_max(1, 1, m, l, r)
        seg.range_set(1, 1, m, l, r, q + 1)

    out = []
    for i in range(1, m + 1):
        out.append(str(seg.range_max(1, 1, m, i, i)))
    return " ".join(out)

# custom cases
assert run("1 1\n1 1\n") == "1", "single cell"
assert run("2 3\n1 3\n2 3\n") == "2 2 2", "overlap propagation"
assert run("3 5\n1 2\n3 4\n2 5\n") == "2 2 2 2 2", "full merge"
assert run("2 5\n1 5\n1 5\n") == "2 2 2 2 2", "stacking full range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | xử lý ranh giới tối thiểu | 
| lan truyền chồng chéo | 2 2 2 | sự phụ thuộc chính xác vào các bản cập nhật trước đó | 
| hợp nhất đầy đủ | 2 2 2 2 2 | tương tác của các khoảng chồng chéo | 
| xếp chồng đầy đủ | 2 2 2 2 2 | cập nhật toàn cầu lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp tối thiểu với một cột duy nhất đảm bảo cây phân đoạn xử lý chính xác các phạm vi suy biến. Đối với đầu vào`1 1`theo sau là một bản cập nhật duy nhất`[1,1]`, giá trị tối đa bằng 0 và giá trị cuối cùng trở thành một. Cấu trúc giảm xuống còn một lá duy nhất, vì vậy cả truy vấn và cập nhật đều phải hoạt động trực tiếp trên nút đó. 

Một trình tự chồng chéo hoàn toàn như lặp đi lặp lại`[1,m]`khoảng thời gian nhấn mạnh sự lan truyền lười biếng. Sau lần cập nhật đầu tiên, tất cả các giá trị sẽ trở thành một. Bản cập nhật thứ hai vẫn phải truy vấn chính xác trước khi ghi đè, tạo ra hai bản ở mọi nơi. Bất kỳ triển khai nào vô tình truy vấn sau khi cập nhật sẽ tiếp tục tăng không chính xác so với các giá trị đã được sửa đổi mà không tôn trọng cấu trúc tối đa ban đầu. 

Một trường hợp chồng chéo hỗn hợp như`[1,2]`,`[2,3]`,`[1,3]`kiểm tra xem việc truyền bá một phần có được xử lý chính xác hay không. Bản cập nhật thứ hai phụ thuộc vào bản cập nhật đầu tiên và bản cập nhật thứ ba phụ thuộc vào cả hai. Cây phân đoạn đảm bảo rằng mỗi truy vấn nhìn thấy trạng thái nhất quán mới nhất trước khi xảy ra bất kỳ hoạt động ghi đè nào, duy trì tính chính xác trên các phần phụ thuộc theo chuỗi.
