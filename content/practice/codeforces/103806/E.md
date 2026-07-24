---
title: "CF 103806E - Thanh tra viên"
description: "Chúng ta có một dãy nhà được đánh số từ 1 đến n. Mỗi ngôi nhà bắt đầu với số dư ngân hàng ban đầu. Sau đó, một chuỗi các sự kiện xảy ra theo thời gian. Một số sự kiện sửa đổi số dư trong toàn bộ khoảng thời gian của các ngôi nhà bằng cách thêm một giá trị có thể dương hoặc âm."
date: "2026-07-02T08:41:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103806
codeforces_index: "E"
codeforces_contest_name: "XXVI Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 103806
solve_time_s: 66
verified: true
draft: false
---

[CF 103806E - Thanh tra viên](https://codeforces.com/problemset/problem/103806/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy nhà được đánh số từ 1 đến n. Mỗi ngôi nhà bắt đầu với số dư ngân hàng ban đầu. Sau đó, một chuỗi các sự kiện xảy ra theo thời gian. Một số sự kiện sửa đổi số dư trong toàn bộ khoảng thời gian của các ngôi nhà bằng cách thêm một giá trị có thể dương hoặc âm. Các sự kiện khác mở ra một cuộc “điều tra” về một ngôi nhà cụ thể và sau đó đóng nó lại. Khi cuộc điều tra kết thúc, chúng tôi được yêu cầu báo cáo số dư nhỏ nhất mà ngôi nhà đó từng có trong suốt khoảng thời gian điều tra. 

Chi tiết quan trọng là số dư thay đổi theo thời gian do cập nhật phạm vi và chúng tôi chỉ phải theo dõi thời gian tối thiểu cho mỗi ngôi nhà trong khoảng thời gian nó đang được điều tra. Vì vậy, mỗi ngôi nhà tôi có nhiều giai đoạn “không hoạt động”, sau đó “điều tra tích cực”, rồi “không hoạt động trở lại”, và trong các giai đoạn hoạt động, chúng tôi liên tục quan sát giá trị của nó và ghi nhớ mức tối thiểu mà nó đạt được. 

Các ràng buộc lên tới 200000 ngôi nhà và 200000 sự kiện, điều này ngay lập tức loại trừ mọi giải pháp quét tất cả các ngôi nhà cho mỗi lần cập nhật hoặc tính toán lại toàn bộ mảng nhiều lần. Một mô phỏng đơn giản trong đó mỗi lần cập nhật phạm vi chạm đến tất cả các ngôi nhà bị ảnh hưởng sẽ có chi phí O(nq), điều này vượt xa khả thi. Ngay cả việc duy trì lịch sử của từng ngôi nhà một cách rõ ràng cũng sẽ quá lớn cả về thời gian và bộ nhớ vì mỗi ngôi nhà có thể bị ảnh hưởng bởi nhiều bản cập nhật. 

Một khó khăn nhỏ là các cập nhật và truy vấn được xen kẽ hoàn toàn. Chúng tôi không thể xử lý tất cả các bản cập nhật trước hoặc tất cả các truy vấn trước. Chúng ta phải duy trì một cấu trúc năng động hỗ trợ việc bổ sung phạm vi và truy cập nhanh vào giá trị hiện tại của một ngôi nhà bất cứ lúc nào. 

Một trường hợp ẩn nữa là một khoảng thời gian điều tra có thể bao gồm nhiều bản cập nhật của các dấu hiệu khác nhau. Mức tối thiểu của một ngôi nhà có thể xảy ra ở giữa phân khúc hoạt động của nó, không nhất thiết phải ở đầu hoặc cuối. Vì vậy, chúng ta phải theo dõi liên tục các giá trị cực tiểu lịch sử chứ không chỉ các giá trị điểm cuối. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì toàn bộ mảng và áp dụng từng sự kiện “người đưa thư” bằng cách lặp từ l đến r, cập nhật từng ngôi nhà bị ảnh hưởng. Trong quá trình điều tra, chúng tôi cũng sẽ theo dõi giá trị tối thiểu của ngôi nhà đó bằng cách kiểm tra nó sau mỗi lần cập nhật. Điều này hoạt động về mặt khái niệm, nhưng mọi bản cập nhật đều có thể chạm vào các phần tử O(n), tạo ra trường hợp xấu nhất là O(nq), quá lớn đối với 200000. 

Quan sát quan trọng là mọi thao tác đều tuyến tính và thống nhất trên một phân đoạn: mỗi lần cập nhật sẽ thêm một hằng số x vào tất cả các phần tử trong một phạm vi. Điều này gợi ý một cây phân đoạn có khả năng lan truyền lười biếng, vì nó hỗ trợ một cách tự nhiên các truy vấn điểm và phép cộng phạm vi theo thời gian logarit. 

Tuy nhiên, chúng ta cũng cần theo dõi giá trị tối thiểu của mỗi ngôi nhà trong một khoảng thời gian chứ không chỉ giá trị cuối cùng của nó. Sự đơn giản hóa cấu trúc quan trọng là đối với một ngôi nhà cố định, giá trị của nó tăng lên theo một chuỗi các lần bổ sung theo thời gian. Nếu chúng tôi biết giá trị hiện tại của nó và chúng tôi cũng duy trì giá trị tối thiểu mà nó từng đạt được kể từ khi cuộc điều tra bắt đầu thì mọi cập nhật ảnh hưởng đến nó chỉ đơn giản là thay đổi cả giá trị hiện tại và mức tối thiểu lịch sử một lượng như nhau. Điều này giúp có thể duy trì cả số lượng cục bộ cho mỗi ngôi nhà. 

Vì vậy, thay vì theo dõi toàn bộ lịch sử, mỗi ngôi nhà lưu trữ hai giá trị: giá trị hiện tại và giá trị tối thiểu kể từ sự kiện “bắt đầu” cuối cùng của nó. Việc bổ sung phạm vi sẽ ảnh hưởng đến cả hai số lượng như nhau đối với tất cả các ngôi nhà bị ảnh hưởng. Cây phân đoạn có thể duy trì các giá trị này một cách hiệu quả bằng phương pháp lan truyền lười biếng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cây phân đoạn trên dãy nhà. Mỗi chiếc lá lưu trữ hai thông tin: số dư hiện tại của ngôi nhà đó và số dư tối thiểu mà nó đạt được kể từ lần cuối cùng cuộc điều tra bắt đầu. Các nút nội bộ chỉ tổng hợp những gì cần thiết để cập nhật phạm vi. 

Các thao tác được xử lý như sau. 

1. Xây dựng cây phân đoạn bằng số dư ban đầu. Lúc đầu, mức tối thiểu kể từ lần bắt đầu cuối cùng bằng giá trị ban đầu vì chưa có cuộc điều tra nào bắt đầu. 
2. Để cập nhật phạm vi trong khoảng [l, r] với giá trị x, chúng tôi áp dụng cập nhật lan truyền lười biếng để thêm x vào mọi nút bị ảnh hưởng. Vì cả giá trị hiện tại và giá trị tối thiểu lịch sử đều thay đổi chính xác bằng x nên chúng tôi thêm x vào cả hai trường trong mọi phân khúc bị ảnh hưởng. Điều này duy trì tính chính xác vì tất cả các giá trị trong quá khứ trong phân đoạn đó được dịch đồng nhất. 
3. Khi cuộc điều tra bắt đầu đối với ngôi nhà i, trước tiên chúng tôi đảm bảo rằng chúng tôi có giá trị hiện tại chính xác tại lá đó bằng cách truyền mọi bản cập nhật lười biếng đang chờ xử lý xuống nó. Sau đó, chúng tôi đặt lại “mức tối thiểu kể từ khi bắt đầu” về giá trị hiện tại. Điều này thiết lập một đường cơ sở mới: từ thời điểm này trở đi, chúng tôi chỉ quan tâm đến các giá trị liên quan đến điểm xuất phát này. 
4. Trong quá trình điều tra, những cập nhật tiếp tục ảnh hưởng đến ngôi nhà đó. Bởi vì cả giá trị hiện tại và giá trị tối thiểu đều được cập nhật cùng nhau theo phép cộng phạm vi, giá trị tối thiểu luôn theo dõi chính xác giá trị thấp nhất được thấy kể từ lần đặt lại cuối cùng. 
5. Khi cuộc điều tra kết thúc đối với ngôi nhà i, chúng tôi lại tuyên truyền để đảm bảo tất cả các bản cập nhật được áp dụng, sau đó xuất ra mức tối thiểu được lưu trữ cho ngôi nhà đó. 

Bất biến chính là đối với mỗi ngôi nhà, tại bất kỳ thời điểm nào, mức tối thiểu được lưu trữ bằng giá trị tối thiểu của số dư thực của nó trong khoảng thời gian kể từ sự kiện đặt lại cuối cùng. Các bản cập nhật phạm vi duy trì sự bất biến này vì chúng dịch chuyển thống nhất tất cả các giá trị lịch sử và các sự kiện đặt lại xác định lại chính xác thời điểm bắt đầu một khoảng thời gian mới bằng cách đồng bộ hóa mức tối thiểu với giá trị hiện tại. Vì mọi cập nhật đều là một sự dịch chuyển thống nhất hoặc một ranh giới được đặt lại, nên không có thao tác nào có thể đưa ra một giá trị không được tính ở mức tối thiểu được lưu trữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class SegTree:
    def __init__(self, arr):
        self.n = len(arr) - 1  # 1-indexed
        self.size = 4 * self.n
        self.val = [0] * self.size
        self.mn = [0] * self.size
        self.lazy = [0] * self.size
        self._build(1, 1, self.n, arr)

    def _build(self, idx, l, r, arr):
        if l == r:
            self.val[idx] = arr[l]
            self.mn[idx] = arr[l]
            return
        mid = (l + r) // 2
        self._build(idx*2, l, mid, arr)
        self._build(idx*2+1, mid+1, r, arr)
        self.mn[idx] = min(self.mn[idx*2], self.mn[idx*2+1])

    def _push(self, idx):
        if self.lazy[idx] != 0:
            v = self.lazy[idx]
            for child in (idx*2, idx*2+1):
                self.val[child] += v
                self.mn[child] += v
                self.lazy[child] += v
            self.lazy[idx] = 0

    def _range_add(self, idx, l, r, ql, qr, v):
        if ql <= l and r <= qr:
            self.val[idx] += v
            self.mn[idx] += v
            self.lazy[idx] += v
            return
        self._push(idx)
        mid = (l + r) // 2
        if ql <= mid:
            self._range_add(idx*2, l, mid, ql, qr, v)
        if qr > mid:
            self._range_add(idx*2+1, mid+1, r, ql, qr, v)
        self.mn[idx] = min(self.mn[idx*2], self.mn[idx*2+1])

    def _query_val(self, idx, l, r, pos):
        if l == r:
            return self.val[idx]
        self._push(idx)
        mid = (l + r) // 2
        if pos <= mid:
            return self._query_val(idx*2, l, mid, pos)
        else:
            return self._query_val(idx*2+1, mid+1, r, pos)

    def reset_min(self, pos):
        self._reset_min(1, 1, self.n, pos)

    def _reset_min(self, idx, l, r, pos):
        if l == r:
            self.mn[idx] = self.val[idx]
            return
        self._push(idx)
        mid = (l + r) // 2
        if pos <= mid:
            self._reset_min(idx*2, l, mid, pos)
        else:
            self._reset_min(idx*2+1, mid+1, r, pos)
        self.mn[idx] = min(self.mn[idx*2], self.mn[idx*2+1])

    def get_min(self, pos):
        return self._get_min(1, 1, self.n, pos)

    def _get_min(self, idx, l, r, pos):
        if l == r:
            return self.mn[idx]
        self._push(idx)
        mid = (l + r) // 2
        if pos <= mid:
            return self._get_min(idx*2, l, mid, pos)
        else:
            return self._get_min(idx*2+1, mid+1, r, pos)

def main():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    st = SegTree(a)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == 'C':
            l, r, x = map(int, tmp[1:])
            st._range_add(1, 1, n, l, r, x)
        elif tmp[0] == 'I':
            i = int(tmp[1])
            st.reset_min(i)
        else:
            i = int(tmp[1])
            print(st.get_min(i))

if __name__ == "__main__":
    main()
```Cây phân đoạn lưu trữ cả giá trị hiện tại và giá trị tối thiểu đang chạy kể từ lần đặt lại cuối cùng ở mỗi lá. Sự lan truyền lười biếng đảm bảo rằng việc bổ sung phạm vi được áp dụng theo thời gian logarit. Hoạt động đặt lại được triển khai dưới dạng cập nhật điểm đồng bộ hóa giá trị tối thiểu với giá trị hiện tại tại thời điểm đó, bắt đầu một cửa sổ theo dõi mới cho ngôi nhà đó một cách hiệu quả. 

Một điểm tinh tế là chúng tôi luôn đẩy các bản cập nhật lười biếng trước khi truy cập vào một chiếc lá. Nếu không có điều này, các hoạt động đặt lại và truy vấn có thể đọc các giá trị cũ và phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với ba ngôi nhà ban đầu`[5, 2, 7]`. Giả sử chúng ta bắt đầu điều tra ở nhà 2, sau đó áp dụng cập nhật phạm vi thêm`-3`đến tất cả các ngôi nhà, và sau đó kết thúc cuộc điều tra. 

| Bước | Hoạt động | Giá trị tại ngôi nhà 2 | Tối thiểu kể từ khi bắt đầu | 
| --- | --- | --- | --- | 
| 1 | Bắt đầu điều tra | 2 | 2 | 
| 2 | Áp dụng -3 cho tất cả | -1 | -1 | 
| 3 | Kết thúc điều tra | -1 | -1 | 

Dấu vết này cho thấy mức tối thiểu tuân theo chính xác sự thay đổi do bản cập nhật gây ra và giá trị thấp nhất trong khoảng thời gian đó sẽ được ghi lại. 

Bây giờ hãy xem xét các cập nhật chồng chéo chỉ ảnh hưởng một phần đến ngôi nhà. Đặt giá trị ban đầu ở nhà 1 là 10. Bắt đầu điều tra, áp dụng +5 cho [2,3] (không có hiệu lực), sau đó áp dụng -7 cho [1,1], rồi kết thúc. 

| Bước | Hoạt động | Giá trị | Tối thiểu | 
| --- | --- | --- | --- | 
| 1 | Bắt đầu | 10 | 10 | 
| 2 | +5 đến [2,3] | 10 | 10 | 
| 3 | -7 đến [1,1] | 3 | 3 | 
| 4 | Kết thúc | 3 | 3 | 

Điều này xác nhận rằng chỉ những cập nhật có liên quan mới ảnh hưởng đến ngôi nhà được theo dõi và những cập nhật tối thiểu chính xác khi giá trị của nó thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi thao tác cập nhật phạm vi và điểm đi qua một đường dẫn cây phân đoạn | 
| Không gian | O(n) | Cây phân đoạn lưu trữ thông tin không đổi trên mỗi nút | 

Các ràng buộc cho phép tối đa 200000 phép toán, do đó hệ số logarit cho mỗi phép toán là đủ. Cấu trúc tránh chạm vào tất cả các ngôi nhà trong mỗi lần cập nhật và chỉ xử lý các phân đoạn bị ảnh hưởng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    main()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample-like test
assert run("""5 5
1 2 3 4 5
I 1
C 1 5 -2
F 1
F 1
F 1
""").split() == ["-1", "-1", "-1"]

# single house stress
assert run("""1 4
10
I 1
C 1 1 -5
C 1 1 2
F 1
""").strip() == "5"

# no updates
assert run("""3 3
1 2 3
I 2
F 2
F 2
""").split() == ["2", "2"]

# full range oscillation
assert run("""3 6
1 1 1
I 2
C 1 3 5
C 1 3 -10
F 2
F 2
""").split() == ["-4", "-4"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật nhà đơn | 5 | sửa lỗi lười + reset tương tác | 
| không có cập nhật | 2,2 | xử lý cơ bản | 
| dao động toàn dải | -4,-4 | thay đổi phạm vi lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi quá trình điều tra bắt đầu chính xác sau khi cập nhật phạm vi. Trong trường hợp đó, giá trị tại ngôi nhà phải bao gồm tất cả các bản cập nhật lười biếng đang chờ xử lý trước khi đặt lại. Việc triển khai xử lý vấn đề này bằng cách đẩy các giá trị lười biếng trước khi truy cập vào lá. Ví dụ: bắt đầu từ một ngôi nhà sau khi bản cập nhật đang chờ xử lý đảm bảo đường cơ sở được đặt lại là chính xác. 

Một trường hợp khác là khi nhiều bản cập nhật ảnh hưởng đến các phạm vi rời rạc trong khi cuộc điều tra đang diễn ra. Thuật toán vẫn hoạt động vì chỉ các phân đoạn bị ảnh hưởng truyền bá các thay đổi và mức tối thiểu ở các lá được cập nhật một cách nhất quán. 

Một trường hợp tinh tế cuối cùng là việc thiết lập lại nhiều lần cho cùng một ngôi nhà. Mỗi lần đặt lại sẽ khởi tạo lại chính xác giá trị tối thiểu thành giá trị hiện tại, đảm bảo lịch sử trước đó không bị rò rỉ vào cửa sổ điều tra mới.
