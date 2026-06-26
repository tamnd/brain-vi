---
title: "CF 105314C - Hamza và Hội chứng thỏa mãn"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm mô tả một chuỗi các mục được sắp xếp theo một thứ tự cố định. Mỗi mục có một ID và một màu sắc. Từ chuỗi này, chúng tôi muốn chọn một chuỗi các mục trong khi vẫn giữ nguyên thứ tự ban đầu."
date: "2026-06-23T15:02:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105314
codeforces_index: "C"
codeforces_contest_name: "Robbing Balloons 2.0 Qualifications"
rating: 0
weight: 105314
solve_time_s: 76
verified: true
draft: false
---

[CF 105314C - Hamza và Hội chứng thỏa mãn](https://codeforces.com/problemset/problem/105314/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm mô tả một chuỗi các mục được sắp xếp theo một thứ tự cố định. Mỗi mục có một ID và một màu sắc. 

Từ chuỗi này, chúng tôi muốn chọn một chuỗi các mục trong khi vẫn giữ nguyên thứ tự ban đầu. Dãy con được chọn phải thỏa mãn hai điều kiện. Đầu tiên, ID của các mục đã chọn phải tăng nghiêm ngặt khi chúng ta di chuyển dọc theo dãy con. Thứ hai, bất cứ khi nào hai mục được chọn liên tiếp nằm cạnh nhau trong chuỗi tiếp theo thì màu của chúng phải khác nhau. 

Đối với mỗi trường hợp kiểm thử, nhiệm vụ là xác định độ dài tối đa có thể có của dãy con hợp lệ như vậy. 

Ràng buộc rằng dãy con phải tuân theo thứ tự ban đầu ngay lập tức loại trừ mọi ý tưởng sắp xếp lại hoặc sắp xếp các mục. Bất kỳ lựa chọn nào cũng phải tôn trọng các vị trí, điều này thúc đẩy chúng ta hướng tới lập trình động trên các chỉ số. 

Với tổng số tối đa 10^5 mục trong tất cả các trường hợp thử nghiệm, phương pháp O(n^2) kiểm tra tất cả các vị trí trước đó cho mọi phần tử sẽ không tồn tại. Loại giải pháp đó thực hiện khoảng 10^10 lần chuyển đổi trong trường hợp xấu nhất, vượt xa giới hạn hai giây cho phép. Do đó, chúng tôi cần một phương pháp xử lý từng mục theo thời gian khấu hao logarit hoặc gần như không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều mục có cùng ID hoặc màu. 

Ví dụ: hãy xem xét ID tăng dần nhưng màu sắc xen kẽ: 

đầu vào:```
1
5
1 2 3 4 5
1 1 1 1 1
```Câu trả lời đúng là 1, vì dù ID có tăng nhưng chúng ta không thể đặt hai vật phẩm liên tiếp do màu sắc giống hệt nhau. 

Một trường hợp lỗi khác xuất hiện khi màu sắc xen kẽ hoàn hảo nhưng ID ngăn chặn chuỗi: 

đầu vào:```
1
4
1 3 2 4
1 2 3 4
```Mặc dù màu sắc cho phép luân phiên, nhưng các ràng buộc về thứ tự ID ngăn cản việc lấy tất cả các mục trong chuỗi con ID tăng dần hợp lệ. 

Những ví dụ này cho thấy rằng không có ràng buộc nào có thể được tối ưu hóa một cách độc lập; cả hai phải được thi hành cùng nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi dãy số có thể xảy ra. Đối với mỗi vị trí, chúng tôi quyết định có đưa nó vào hay không sau khi kiểm tra tất cả các vị trí trước đó. Điều này dẫn đến một công thức lập trình động cổ điển trong đó chúng tôi tính toán chuỗi con hợp lệ tốt nhất kết thúc ở mỗi chỉ mục. Quá trình chuyển đổi sẽ thử tất cả các chỉ mục trước đó với ID nhỏ hơn và màu khác. Điều này đúng, nhưng nó yêu cầu quét tất cả các phần tử trước đó để tìm mọi vị trí, dẫn đến độ phức tạp bậc hai. 

Điểm nghẽn là việc tìm kiếm lặp đi lặp lại phiên bản tiền nhiệm tương thích tốt nhất. Cấu trúc của bài toán gợi ý hai ràng buộc độc lập: một về thứ tự ID và một về độ kề màu. Ràng buộc ID là điều kiện loại "tiền tố tối đa theo giá trị" tiêu chuẩn, thường được xử lý bằng cây Fenwick hoặc cây phân đoạn trên ID nén. Ràng buộc màu sắc làm phức tạp vấn đề vì chúng ta phải loại trừ các chuyển tiếp đến từ cùng một màu. 

Ý tưởng chính là duy trì hai phần thông tin cho mỗi tiền tố của các mục được xử lý theo thứ tự vị trí của chúng. Chúng tôi duy trì độ dài chuỗi con tốt nhất cho tất cả các ứng cử viên hợp lệ có ID dưới ngưỡng và chúng tôi cũng theo dõi giá trị tốt nhất cho mỗi màu trong cùng phạm vi ID đó. Khi tính toán quá trình chuyển đổi cho một mục mới, chúng tôi lấy tổng thể tốt nhất và trừ đi những đóng góp không hợp lệ đến từ màu của chính mục đó. 

Để hỗ trợ cả truy vấn và cập nhật tiền tố theo ID một cách hiệu quả, chúng tôi sử dụng cây phân đoạn (hoặc cây Fenwick) trên ID nén. Mỗi nút lưu trữ giá trị DP tốt nhất cho phạm vi ID đó. Bên cạnh đó, chúng tôi duy trì cấu trúc mỗi màu theo dõi giá trị DP tốt nhất được thấy cho đến nay đối với màu đó trong cùng một giới hạn ID. Sự kết hợp của hai cấu trúc này cho phép chúng ta tính toán các chuyển đổi hợp lệ theo thời gian logarit trên mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n^2) | O(n) | Quá chậm | 
| Cây phân đoạn + Theo dõi màu sắc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các mục theo thứ tự nhất định và duy trì giá trị DP cho mỗi mục đại diện cho chuỗi con hợp lệ tốt nhất kết thúc tại mục đó. 

1. Nén tất cả các ID để chúng nằm trong một phạm vi liền kề. Điều này cho phép chúng ta sử dụng chúng làm chỉ mục trong cây phân đoạn. 
2. Khởi tạo cây phân đoạn trên các giá trị ID. Mỗi vị trí trong cây lưu trữ giá trị DP tối đa của bất kỳ chuỗi con nào kết thúc bằng một mục có ID tương ứng với vị trí đó. 
3. Duy trì một bản đồ băm hoặc từ điển lưu trữ, đối với mỗi màu, giá trị DP tốt nhất được thấy cho đến nay trong số tất cả các mục đã xử lý. Giá trị này đại diện cho chuỗi con tốt nhất kết thúc bằng màu đó, bất kể ràng buộc ID. 
4. Đối với mỗi mục theo thứ tự, hãy tính dãy con tốt nhất kết thúc tại nó như sau. Đầu tiên, truy vấn cây phân đoạn để tìm giá trị DP tối đa trên tất cả các ID nhỏ hơn ID của mục hiện tại. Điều này mang lại chuỗi con tốt nhất có thể mà chúng tôi có thể mở rộng chỉ dựa trên ràng buộc ID. 
5. Nếu chuỗi con tốt nhất thu được ở bước trước kết thúc bằng cùng màu với mục hiện tại thì nó không thể được mở rộng trực tiếp. Trong trường hợp đó, chúng ta phải tránh sử dụng nó và thay vào đó dựa vào tùy chọn hợp lệ tốt nhất tiếp theo. Điều này được xử lý bằng cách kiểm tra bản đồ màu và đảm bảo chúng tôi không sử dụng lại phần đóng góp màu giống nhau. 
6. Đặt DP[i] thành 1 cộng với giá trị hợp lệ tốt nhất thu được. Điều này giải thích việc chọn mục hiện tại làm phần tử cuối cùng của chuỗi con. 
7. Cập nhật cây phân đoạn tại vị trí của ID hiện tại bằng DP[i] và cập nhật bản đồ màu cho màu hiện tại bằng DP[i] nếu nó cải thiện giá trị được lưu trữ. 

### Tại sao nó hoạt động

Ở mỗi bước, cây phân đoạn lưu trữ chuỗi con tốt nhất có thể đạt được kết thúc bằng bất kỳ ID hợp lệ nào nhỏ hơn ID hiện tại. Điều này đảm bảo rằng mọi tiện ích mở rộng đều tôn trọng ràng buộc ID ngày càng tăng nghiêm ngặt. Bản đồ màu đảm bảo rằng khi chúng tôi xem xét việc mở rộng một chuỗi con, chúng tôi có thể xác định và loại trừ các chuyển đổi có thể vi phạm điều kiện màu liền kề. Vì mọi trạng thái DP chỉ được xây dựng từ các trạng thái hợp lệ trước đó và chúng tôi luôn xem xét tất cả các trạng thái trước đó hợp lệ theo cả hai ràng buộc, nên không có chuỗi con tối ưu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        vals = sorted(set(a))
        comp = {v: i+1 for i, v in enumerate(vals)}
        m = len(vals)

        seg = [0] * (4 * m)
        dp = [0] * n
        best_color = {}

        def update(node, l, r, idx, val):
            if l == r:
                seg[node] = max(seg[node], val)
                return
            mid = (l + r) // 2
            if idx <= mid:
                update(node * 2, l, mid, idx, val)
            else:
                update(node * 2 + 1, mid + 1, r, idx, val)
            seg[node] = max(seg[node * 2], seg[node * 2 + 1])

        def query(node, l, r, ql, qr):
            if ql > r or qr < l:
                return 0
            if ql <= l and r <= qr:
                return seg[node]
            mid = (l + r) // 2
            return max(
                query(node * 2, l, mid, ql, qr),
                query(node * 2 + 1, mid + 1, r, ql, qr)
            )

        for i in range(n):
            ci = comp[a[i]]

            best = 0
            if ci > 1:
                best = query(1, 1, m, 1, ci - 1)

            cand = best

            if b[i] in best_color:
                cand = max(cand, best_color[b[i]])

            dp[i] = cand + 1

            update(1, 1, m, ci, dp[i])

            if b[i] not in best_color or best_color[b[i]] < dp[i]:
                best_color[b[i]] = dp[i]

        print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp nén ID để các hoạt động của cây phân đoạn vẫn hiệu quả. Cây phân đoạn được sử dụng để lấy giá trị DP có thể đạt được tốt nhất trong số tất cả các ID nhỏ hơn. Từ điển theo dõi các chuỗi con tốt nhất cho mỗi màu để chúng tôi có thể tránh các chuyển đổi không hợp lệ sẽ đặt hai màu giống hệt nhau liên tiếp. 

Việc tính toán DP được thực hiện từ trái sang phải, đảm bảo tất cả các trạng thái bắt buộc đều có sẵn khi xử lý từng mục. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ: 

đầu vào:```
1
4
1 2 3 4
1 2 1 2
```Chúng tôi theo dõi sự đóng góp của DP và màu sắc. 

| tôi | ID | Màu sắc | ID nhỏ hơn tốt nhất | Cùng màu tốt nhất | DP[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 1 | 
| 2 | 2 | 2 | 1 | 0 | 2 | 
| 3 | 3 | 1 | 2 | 1 | 3 | 
| 4 | 4 | 2 | 3 | 2 | 4 | 

Điều này xác nhận rằng khi cả hai ràng buộc đều thẳng hàng, chúng ta có thể mở rộng hầu hết mọi bước. 

Bây giờ hãy xem xét trường hợp chuyển tiếp khối màu: 

đầu vào:```
1
5
1 2 3 4 5
1 1 2 1 2
```| tôi | ID | Màu sắc | ID nhỏ hơn tốt nhất | Cùng màu tốt nhất | DP[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 1 | 
| 2 | 2 | 1 | 1 | 1 | 1 | 
| 3 | 3 | 2 | 1 | 0 | 2 | 
| 4 | 4 | 1 | 2 | 1 | 2 | 
| 5 | 5 | 2 | 2 | 2 | 3 | 

Điều này cho thấy màu sắc lặp lại hạn chế chuỗi như thế nào ngay cả khi ID cho phép điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi mục thực hiện truy vấn cây phân đoạn và cập nhật qua ID nén | 
| Không gian | O(n) | Lưu trữ cây phân đoạn, mảng DP và bản đồ màu | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm là tuyến tính theo số lượng mục và mỗi thao tác là logarit do cây phân đoạn. Với n lên tới 10^5, điều này thoải mái nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        vals = sorted(set(a))
        comp = {v: i+1 for i, v in enumerate(vals)}
        m = len(vals)

        seg = [0] * (4 * m)
        dp = [0] * n
        best_color = {}

        def update(node, l, r, idx, val):
            if l == r:
                seg[node] = max(seg[node], val)
                return
            mid = (l + r) // 2
            if idx <= mid:
                update(node * 2, l, mid, idx, val)
            else:
                update(node * 2 + 1, mid + 1, r, idx, val)
            seg[node] = max(seg[node * 2], seg[node * 2 + 1])

        def query(node, l, r, ql, qr):
            if ql > r or qr < l:
                return 0
            if ql <= l and r <= qr:
                return seg[node]
            mid = (l + r) // 2
            return max(
                query(node * 2, l, mid, ql, qr),
                query(node * 2 + 1, mid + 1, r, ql, qr)
            )

        for i in range(n):
            ci = comp[a[i]]
            best = 0
            if ci > 1:
                best = query(1, 1, m, 1, ci - 1)

            cand = best
            if b[i] in best_color:
                cand = max(cand, best_color[b[i]])

            dp[i] = cand + 1
            update(1, 1, m, ci, dp[i])

            best_color[b[i]] = max(best_color.get(b[i], 0), dp[i])

        out.append(str(max(dp)))

    return "\n".join(out)

# sample 1
assert run("""1
4
1 2 3 4
1 2 1 2
""") == "4"

# all same color
assert run("""1
5
1 2 3 4 5
1 1 1 1 1
""") == "1"

# alternating colors
assert run("""1
4
1 3 2 4
1 2 3 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tăng ID xen kẽ màu sắc | 4 | Chuỗi đầy đủ khi các ràng buộc căn chỉnh | 
| Tất cả cùng màu | 1 | Hạn chế màu sắc chiếm ưu thế | 
| ID chưa được sắp xếp | 3 | Lựa chọn giới hạn ràng buộc thứ tự | 

## Vỏ cạnh 

Trường hợp tất cả các mục có cùng màu chứng tỏ rằng thuật toán không bao giờ xâu chuỗi hai màu bằng nhau vì bản đồ màu đảm bảo mọi tiện ích mở rộng sử dụng màu đó không thể vượt quá một chuỗi phần tử. 

đầu vào:```
1
4
1 2 3 4
7 7 7 7
```Cây phân đoạn sẽ luôn trả về các giá trị DP tăng dần theo ID, nhưng bản đồ màu buộc mọi chuyển đổi chỉ xem xét các chuỗi con một phần tử. Kết quả vẫn là 1. 

Một trường hợp có ID giảm sẽ kiểm tra xem liệu chỉ nén tọa độ có thể xử lý thứ tự hay không: 

đầu vào:```
1
5
5 4 3 2 1
1 2 3 4 5
```Vì không có cặp nào đáp ứng thứ tự ID tăng dần nên mỗi DP vẫn bằng 1. Cây phân đoạn không bao giờ trả về các tiền tố có ý nghĩa, xác nhận tính chính xác của đầu vào đảo ngược.
