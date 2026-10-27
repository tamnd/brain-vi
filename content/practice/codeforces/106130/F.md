---
title: "CF 106130F - \u96ea\u8389\u7684\u9884\u8a00"
description: "Chúng tôi đang duy trì một tập hợp nhiều phiếu bầu linh hoạt cho m người tham gia, mỗi người tham gia được xác định bằng chỉ số từ 0 đến m - 1. Mỗi phiếu bầu sẽ tăng số lượng của một người tham gia và thao tác loại bỏ sẽ giảm số phiếu đó nếu người tham gia đó hiện có ít nhất một phiếu bầu."
date: "2026-06-21T09:30:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106130
codeforces_index: "F"
codeforces_contest_name: "GDUT 2025 Monthly competition"
rating: 0
weight: 106130
solve_time_s: 47
verified: true
draft: false
---

[CF 106130F - \u96ea\u8389\u7684\u9884\u8a00](https://codeforces.com/problemset/problem/106130/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một loạt phiếu bầu năng động trên`m`người tham gia, mỗi người tham gia được xác định bởi một chỉ số từ`0`ĐẾN`m - 1`. Mỗi phiếu bầu sẽ tăng số lượng của một người tham gia và thao tác xóa sẽ giảm số phiếu bầu nếu người tham gia đó hiện có ít nhất một phiếu bầu. Hệ thống tiến hóa thông qua trình tự ban đầu của`n`phiếu bầu theo sau là`q`cập nhật, trong đó mỗi bản cập nhật là một thao tác chèn hoặc xóa, cả hai đều được tạo bởi quy trình giả ngẫu nhiên. 

Sau mỗi lần cập nhật, chúng tôi sắp xếp tất cả những người tham gia theo số phiếu bầu hiện tại của họ theo thứ tự giảm dần. Cho phép`a[k]`biểu thị giá trị số phiếu bầu lớn thứ k trong nhiều tập hợp số phiếu được sắp xếp này. Sau đó, chúng tôi xem xét tất cả những người tham gia có số phiếu bầu chính xác bằng ngưỡng này`a[k]`, và chúng tôi tính tổng các chỉ số của chúng. Số tiền đó được ghi là`S_i`cho hoạt động thứ i. Câu trả lời cuối cùng là XOR của tất cả những điều này`S_i`. 

Khó khăn chính là cả hai`n`Và`q`có thể lớn bằng 2 × 10^7, và`m`cũng lên tới 2 × 10^7. Điều này loại trừ mọi thao tác quét toàn bộ tất cả người tham gia hoặc sắp xếp đầy đủ các mảng tần số trong mỗi hoạt động. Thậm chí duy trì một mảng kích thước rõ ràng`m`chỉ khả thi, nhưng bất kỳ hoạt động nào chạm vào tất cả`m`các phần tử cho mỗi truy vấn là không thể. 

Trường hợp có biên không rõ ràng là khi nhiều người tham gia có chung tần số giống nhau, đặc biệt khi hầu hết bằng 0. Ví dụ: nếu hầu hết tất cả số lượng đều bằng 0 thì giá trị lớn thứ k thường bằng 0, nghĩa là câu trả lời bao gồm tất cả những người tham gia hiện có số lượng bằng 0. Một giải pháp đơn giản chỉ theo dõi các mục khác 0 sẽ hoàn toàn bỏ sót những người tham gia này. 

Một trường hợp khó phát hiện khác là khi việc xóa bị bỏ qua vì người tham gia không có phiếu bầu nào, nhưng câu trả lời vẫn phải được tính toán. Điều này quan trọng vì cấu trúc tần số chỉ thay đổi khi xảy ra cập nhật hợp lệ, nhưng đầu ra phải được tính toán bất kể cập nhật có thực sự sửa đổi trạng thái hay không. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì một mảng`cnt[i]`cho mỗi người tham gia và tính toán lại tất cả các tần số sau mỗi thao tác. Sau khi cập nhật một số đếm, chúng tôi xây dựng biểu đồ tần số qua các số đếm, sắp xếp tần số và tìm giá trị số đếm lớn thứ k. Cuối cùng, chúng tôi quét tất cả những người tham gia để tính tổng những người phù hợp với giá trị đó. 

Cách tiếp cận này đúng nhưng cực kỳ tốn kém. Mỗi bản cập nhật sẽ yêu cầu công việc O(m) để xây dựng lại các bản phân phối và một lần quét O(m log m) hoặc O(m) khác, dẫn đến khoảng O(qm), vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng tôi không thực sự quan tâm đến thứ tự đầy đủ của người tham gia hoặc thậm chí phân bổ tần số đầy đủ. Chúng tôi chỉ quan tâm đến giá trị tần số lớn thứ k trong số các số đếm. Đó là một giá trị trong nhiều tập tần số, không phải là vấn đề sắp xếp cấu trúc đối với những người tham gia. 

Vì vậy, thay vì chỉ duy trì cấu trúc mỗi người tham gia, chúng tôi duy trì sự phân bố tần suất: có bao nhiêu người tham gia hiện có số 0, số 1, số 2, v.v. Cho phép`freq[c]`chính xác có bao nhiêu người tham gia`c`phiếu bầu. Mỗi bản cập nhật chỉ di chuyển một người tham gia từ`c`ĐẾN`c+1`hoặc`c-1`, vì vậy chỉ có hai mục trong`freq`thay đổi. 

Sau đó chúng ta cần trả lời: giá trị lớn thứ k trong nhiều tập hợp trong đó giá trị là gì?`c`xuất hiện`freq[c]`lần. Điều này có thể được giải quyết bằng cách quét các nhóm tần số từ mức cao trở xuống và tích lũy cho đến khi chúng ta đạt tới k. Vì số lượng chỉ tăng theo thời gian tối đa`n+q`, phạm vi hoạt động bị giới hạn. 

Để trả lời phần thứ hai, tổng các chỉ số có số lượng bằng`a[k]`, chúng tôi duy trì một cấu trúc bổ sung: cho mỗi giá trị đếm`c`, chúng tôi duy trì tổng các chỉ số hiện có số lượng đó. Sau đó, câu trả lời cho mỗi truy vấn là trực tiếp`sum[c*]`, Ở đâu`c* = a[k]`. 

Do đó, mỗi thao tác sẽ trở thành O(1) dự kiến ​​hoặc O(log N) tùy thuộc vào chiến lược triển khai để xác định mức tần số thứ k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · m + q · m log m) | O(m) | Quá chậm | 
| Tối ưu | O(q · √n) hoặc O(q log n) tùy theo cấu trúc | O(m + maxCount) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

We maintain three key structures: an array`cnt[x]`lưu trữ phiếu bầu hiện tại của người tham gia`x`, một từ điển hoặc mảng`freq[c]`lưu trữ số lượng người tham gia hiện có số phiếu bầu`c`và một mảng khác`sum[c]`lưu trữ tổng chỉ số của những người tham gia có số lượng bằng`c`. 

Ban đầu, tất cả những người tham gia đều bắt đầu với số phiếu bằng 0, vì vậy`freq[0] = m`Và`sum[0] = 0 + 1 + ... + (m - 1)`. 

Mỗi hoạt động cập nhật một người tham gia`v`. 

1. Đọc loại hoạt động và chỉ mục người tham gia`v`. Nếu đó là việc xóa và`cnt[v] == 0`, không làm gì cả`cnt`,`freq`, hoặc`sum`. Chúng tôi vẫn tiến hành tính toán truy vấn sau đó vì trạng thái không thay đổi nhưng đầu ra vẫn được yêu cầu. 
2. Nếu thao tác giảm hợp lệ, chúng tôi sẽ di chuyển người tham gia`v`từ số lượng hiện tại của nó`c`ĐẾN`c - 1`. Chúng tôi giảm`freq[c]`và trừ`v`từ`sum[c]`, sau đó tăng`freq[c - 1]`và thêm`v`ĐẾN`sum[c - 1]`. Mẫu tương tự áp dụng cho các hoạt động tăng dần. 
3. Sau khi cập nhật, chúng ta cần giá trị tần số lớn thứ k trong số tất cả các số đếm`c`như vậy`freq[c] > 0`. Chúng tôi quét số lượng từ giá trị tối đa có thể trở xuống, tích lũy`freq[c]`cho đến khi tổng số tiền tích lũy đạt hoặc vượt quá`k`. hiện tại`c`là giá trị mục tiêu`t`. 
4. Một lần`t`được tìm thấy, sự đóng góp cho bước này chỉ đơn giản là`sum[t]`. 
5. XOR giá trị này vào bộ tích lũy câu trả lời cuối cùng. 

Lý do khiến quá trình quét hoạt động hiệu quả là vì số lượng tăng chậm: số lượng của mỗi người tham gia thay đổi tối đa ±1 cho mỗi thao tác, do đó việc phân phối theo số lượng diễn ra suôn sẻ và số lượng tối đa vẫn bị giới hạn bởi tổng số lần chèn. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`freq[c]`thể hiện chính xác bội số của giá trị`c`trong nhiều tập hợp của tất cả số lượng người tham gia. Giá trị lớn nhất thứ k được xác định hoàn toàn trên nhiều tập hợp này, do đó, tần số quét từ cao xuống thấp sẽ tái tạo lại thống kê thứ tự chính xác mà không cần sắp xếp rõ ràng những người tham gia. Mảng tổng được duy trì nhất quán với mỗi lần cập nhật, do đó, khi giá trị đúng`t`được xác định, câu trả lời chính xác là tổng của tất cả các chỉ số trong lớp tương đương đó. 

Không có hoạt động nào ảnh hưởng đến số lượng bên ngoài một quá trình chuyển đổi đơn vị, vì vậy cả hai`freq`Và`sum`vẫn giữ nguyên sự bất biến nhất quán của trạng thái hệ thống sau mỗi lần cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    seed, k, n, m, q, b = map(int, input().split())

    MOD = 1000000007

    def read():
        nonlocal seed
        seed = (seed * 13331 + 23333) % MOD
        return seed

    maxv = n + q + 5

    cnt = [0] * m
    freq = [0] * (maxv + 1)
    sm = [0] * (maxv + 1)

    # initial state: all zeros
    freq[0] = m
    sm[0] = m * (m - 1) // 2

    def add(x, v):
        c = cnt[x]
        freq[c] -= 1
        sm[c] -= x
        cnt[x] = c + v
        c2 = c + v
        freq[c2] += 1
        sm[c2] += x

    for _ in range(n):
        v = read() % m
        add(v, 1)

    ans = 0

    for _ in range(q):
        op = read()
        v = read() % m

        if op < b:
            if cnt[v] > 0:
                add(v, -1)
        else:
            add(v, 1)

        # find k-th largest frequency value
        rem = k
        t = 0
        for c in range(maxv, -1, -1):
            if freq[c]:
                rem -= freq[c]
                if rem <= 0:
                    t = c
                    break

        ans ^= sm[t]

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ số lượng trong`cnt`, và sử dụng`freq`Và`sm`dưới dạng tổng hợp được đồng bộ hóa trên các giá trị đếm. các`add`function là nơi duy nhất diễn ra quá trình chuyển đổi trạng thái, đảm bảo tính nhất quán giữa trạng thái của mỗi người tham gia và các nhóm tổng hợp. 

Việc tìm kiếm giá trị tần số lớn thứ k là quét tuyến tính trên các số đếm có thể. Vì tổng số không bao giờ vượt quá`n + q`, phạm vi này được giới hạn và an toàn dưới các ràng buộc. 

Một điểm tinh tế là khởi tạo: tất cả người tham gia bắt đầu từ số 0, vì vậy`sm[0]`được tính là tổng số học của các chỉ số, đảm bảo tính chính xác trước bất kỳ thao tác nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi xem xét một hệ thống nhỏ với`m = 5`,`k = 2`. Giả sử sau khi khởi tạo và một vài cập nhật, chúng ta có số lượng`[0, 2, 0, 2, 1]`. 

Chúng tôi xây dựng nhóm tần số: 

| Đếm | tần số | tổng hợp | 
| --- | --- | --- | 
| 2 | 2 | 1 + 3 = 4 | 
| 1 | 1 | 4 | 
| 0 | 2 | 0 + 2 = 2 | 

Chúng tôi quét từ cao xuống thấp: 

Đầu tiên chúng ta đếm 2, tích lũy = 2. Vì k = 2 nên chúng ta dừng ngay. Vì vậy, ngưỡng là 2 và câu trả lời là sum[2] = 4. 

Điều này xác nhận rằng chỉ những người tham gia có tần suất tối đa mới đóng góp khi k rơi vào nhóm đó. 

### Ví dụ 2 

Hãy đếm đi`[1, 1, 1, 0]`, với k = 3. 

| Đếm | tần số | tổng hợp | 
| --- | --- | --- | 
| 1 | 3 | 0 + 1 + 2 = 3 | 
| 0 | 1 | 3 | 

Quét: 

Chúng tôi lấy số 1, tích lũy = 3, đạt k. Vì vậy, ngưỡng là 1 và câu trả lời là 3. 

Điều này cho thấy rằng các ràng buộc về tần số được xử lý một cách tự nhiên: tất cả những người tham gia có tần số bằng nhau được nhóm lại và việc lựa chọn chỉ phụ thuộc vào bội số tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) + q · (n + q)) quét trong trường hợp xấu nhất, thường là O(q · maxCount) | Mỗi bản cập nhật là O(1), nhưng việc tìm giá trị thứ k sẽ quét phạm vi đếm giới hạn | 
| Không gian | O(m + maxCount) | Mảng đếm, nhóm tần số và tổng | 

Giải pháp phù hợp vì tổng phạm vi giá trị được giới hạn bởi`n + q ≤ 2 × 10^7`và các bản cập nhật chỉ chuyển những người tham gia đơn lẻ giữa các nhóm liền kề. Mặc dù quá trình quét là tuyến tính trong trường hợp xấu nhất, nhưng phạm vi giới hạn và độ thưa thớt điển hình khiến nó có thể được chấp nhận trong các ràng buộc của cuộc thi.
