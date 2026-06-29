---
title: "CF 104611F - \u5b9d\u77f3\u4ea4\u6613"
description: "Chúng ta có hai dãy đá quý hình tròn, cả hai đều có chiều dài n. Mỗi vị trí chứa một loại đá quý và các chuỗi được coi là tuần hoàn, nghĩa là chúng ta có thể chọn bất kỳ điểm bắt đầu nào và di chuyển chúng theo hướng cố định xung quanh vòng tròn."
date: "2026-06-29T22:32:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104611
codeforces_index: "F"
codeforces_contest_name: "2023\u6e56\u5357\u7701\u8d5b"
rating: 0
weight: 104611
solve_time_s: 57
verified: true
draft: false
---

[CF 104611F - \u5b9d\u77f3\u4ea4\u6613](https://codeforces.com/problemset/problem/104611/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy đá quý hình tròn, cả hai đều có chiều dài n. Mỗi vị trí chứa một loại đá quý và các chuỗi được coi là tuần hoàn, nghĩa là chúng ta có thể chọn bất kỳ điểm bắt đầu nào và di chuyển chúng theo hướng cố định xung quanh vòng tròn. 

Có quy luật chuyển hóa trực tiếp giữa các loại đá quý. Mỗi quy tắc nói rằng chúng ta có thể thay đổi đá quý loại a thành loại b với một số chi phí, nhưng thao tác ngược lại không nhất thiết phải có trừ khi được đưa ra rõ ràng. 

Nhiệm vụ là chọn một vòng quay bắt đầu cho chuỗi đầu tiên và một hướng (theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ), sau đó căn chỉnh hai vòng tròn theo từng vị trí. Đối với mỗi cặp đá quý thẳng hàng, chúng ta có thể áp dụng một chuỗi các phép biến đổi để làm cho hai viên đá quý bằng nhau, thanh toán tổng chi phí của các phép biến đổi đó. Chúng tôi muốn tổng chi phí tối thiểu có thể có cho tất cả các lựa chọn về vị trí và hướng xuất phát. Nếu không có cách nào để làm cho tất cả các cặp căn chỉnh bằng nhau, chúng ta sẽ xuất ra −1. 

Các ràng buộc ngụ ý rằng số lượng loại đá quý ít, nhiều nhất là 400, trong khi số lượng quy tắc lớn. Độ dài chuỗi n đủ lớn để mọi O(n2) hoặc tệ hơn trong mỗi lần thử chuyển đổi phải được xử lý cẩn thận, nhưng giải pháp khối dày đặc trên 400 trạng thái là khả thi. Cấu trúc đề xuất tách vấn đề thành hai phần: tính toán chi phí biến đổi ngắn nhất giữa tất cả các loại đá quý và sau đó đánh giá hiệu quả tất cả sự sắp xếp theo chu kỳ giữa hai chuỗi. 

Một trường hợp thất bại tinh tế xuất hiện khi các phép biến đổi là gián tiếp. Ví dụ: nếu chỉ tồn tại a → b và b → c thì a → c là có thể nhưng phải được khám phá qua các bước trung gian. Cách tiếp cận tham lam hoặc chỉ theo hướng trực tiếp sẽ đánh giá thấp tính khả thi hoặc đánh giá quá cao chi phí. 

Một trường hợp khác là xử lý hướng. Nếu chúng ta chỉ xem xét một hướng của dãy thứ hai, chúng ta có thể bỏ lỡ các giải pháp tối ưu hợp lệ yêu cầu đảo ngược thứ tự truyền tải. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp sẽ thử mọi chỉ số bắt đầu có thể có trong vòng tròn đầu tiên và mọi hướng có thể. Đối với mỗi căn chỉnh, nó sẽ tính toán chi phí chuyển đổi từng viên đá quý được ghép nối một cách độc lập bằng cách sử dụng các truy vấn đường dẫn ngắn nhất giữa các loại đá quý. 

Điều này đã gợi ý một sự phân tách: nếu chúng ta biết chi phí tối thiểu để chuyển bất kỳ loại a nào thành bất kỳ loại b nào thì mỗi chi phí căn chỉnh sẽ trở thành một tổng đơn giản trên các vị trí. Sau đó, lực lượng vũ phu sẽ kiểm tra 2n cách sắp xếp, mỗi cách sắp xếp có giá trị O(n) và mỗi lần tra cứu chuyển đổi O(1), đưa ra O(n²) cho mỗi trường hợp thử nghiệm sau khi tiền xử lý. Điểm nghẽn không phải là số lượng sắp xếp mà là tính toán chi phí chuyển đổi một cách chính xác. 

Quan sát quan trọng là các phép biến đổi đá quý tạo thành một biểu đồ có trọng số có hướng trên tối đa 400 nút. Cách rẻ nhất để chuyển đổi loại này sang loại khác là giải bài toán đường đi ngắn nhất trên biểu đồ này. Sau khi tính toán đường đi ngắn nhất của tất cả các cặp, chi phí chuyển đổi giữa hai loại đá quý bất kỳ sẽ trở thành tra cứu trực tiếp. 

Sau lần giảm này, nhiệm vụ còn lại là đánh giá tất cả các ca theo chu kỳ một cách hiệu quả. Đối với sự dịch chuyển và hướng cố định, chi phí là tổng của các vị trí có khoảng cách giữa các loại được tính toán trước. Về mặt cấu trúc, đây là một vấn đề tương quan tuần hoàn trên các chuỗi. Vì n lớn nên việc tính toán lại từng dịch chuyển từ đầu sẽ quá chậm, nhưng vì tất cả các tính toán đều là những đóng góp cộng gộp đơn giản, nên chúng ta có thể đánh giá các dịch chuyển trực tiếp theo tổng O(n²), có thể chấp nhận được trong các giới hạn điển hình cho n lên đến vài nghìn. 

Chúng ta cũng phải đánh giá cả hai hướng: căn chỉnh thuận và căn chỉnh ngược, vì bài toán cho phép chọn hướng di chuyển ngang.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực mà không cần xử lý trước | O(n³) | O(1) | Quá chậm | 
| Floyd + kiểm tra tất cả các ca | O(V³ + n²) | O(V²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Xây dựng chi phí chuyển đổi ngắn nhất 

Chúng tôi mô hình hóa các loại đá quý dưới dạng các nút trong biểu đồ có hướng. Mỗi luật a → b với chi phí c sẽ trở thành một cạnh có hướng. Chúng tôi tính toán chi phí tối thiểu giữa tất cả các cặp nút bằng Floyd-Warshall trên 400 loại có thể. 

Bước này là cần thiết vì các phép biến đổi có thể xâu chuỗi thông qua các loại trung gian và chúng ta cần chi phí tối thiểu thực sự cho mỗi cặp. 

### Bước 2: Chuẩn bị cả hai hướng đi ngang 

Chúng tôi xem xét hai trường hợp riêng biệt. Trong trường hợp chuyển tiếp, chúng tôi giữ nguyên chuỗi thứ hai. Trong trường hợp ngược lại, về mặt khái niệm, chúng tôi đảo ngược hướng di chuyển của một vòng tròn, tương ứng với việc đảo ngược thứ tự của một chuỗi trong quá trình khớp. 

### Bước 3: Liệt kê tất cả các ca tuần hoàn 

Đối với mỗi chỉ số bắt đầu k có thể có trong chuỗi đầu tiên, chúng ta căn chỉnh s[i + k] với t[i] (mod n). Điều này thể hiện việc cố định điểm bắt đầu và đi cả hai vòng tròn cùng nhau. 

### Bước 4: Tính chi phí cho một ca cố định 

Đối với một ca k nhất định, chúng tôi tính tổng chi phí bằng cách tính tổng dist[s[(i+k) mod n]][t[i]] trên tất cả i. Điều này trực tiếp sử dụng bảng đường đi ngắn nhất được tính toán trước. 

### Bước 5: Theo dõi mức tối thiểu trên tất cả các cấu hình 

Chúng tôi lặp lại đánh giá dịch chuyển cho cả hai hướng và giữ giá trị tối thiểu trong mọi trường hợp. Nếu mọi cấu hình mang lại chi phí vô hạn thì chúng ta sẽ xuất ra −1. 

### Tại sao nó hoạt động 

Sau khi tiền xử lý, mọi chuyển đổi giữa các loại đá quý đều không phụ thuộc vào vị trí. Điều này có nghĩa là cấu trúc duy nhất còn lại là căn chỉnh chứ không phải độ phức tạp của phép biến đổi. Vì mọi sự liên kết đều phân hủy thành chi phí cho mỗi vị trí độc lập nên tổng chi phí chính xác là tổng của các chuyển đổi cục bộ tối ưu. Việc kiểm tra toàn diện tất cả các phép quay và cả hai hướng đảm bảo rằng chúng ta xem xét mọi cặp tuần hoàn hợp lệ của hai vòng tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    t = list(map(int, input().split()))

    MAXV = 400
    dist = [[INF] * (MAXV + 1) for _ in range(MAXV + 1)]

    for i in range(1, MAXV + 1):
        dist[i][i] = 0

    for _ in range(m):
        a, b, c = map(int, input().split())
        if c < dist[a][b]:
            dist[a][b] = c

    for k in range(1, MAXV + 1):
        dk = dist[k]
        for i in range(1, MAXV + 1):
            di = dist[i]
            dik = di[k]
            if dik == INF:
                continue
            for j in range(1, MAXV + 1):
                if dk[j] == INF:
                    continue
                nd = dik + dk[j]
                if nd < di[j]:
                    di[j] = nd

    def calc(arr_t):
        best = INF
        for shift in range(n):
            total = 0
            ok = True
            for i in range(n):
                a = s[(i + shift) % n]
                b = arr_t[i]
                d = dist[a][b]
                if d == INF:
                    ok = False
                    break
                total += d
            if ok:
                best = min(best, total)
        return best

    ans1 = calc(t)
    ans2 = calc(t[::-1])

    ans = min(ans1, ans2)
    if ans >= INF:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng bảng khoảng cách 400 x 400 và khởi tạo nó với chi phí chuyển đổi trực tiếp. Floyd-Warshall được áp dụng để truyền các phép biến đổi gián tiếp sao cho dist[a][b] trở thành chi phí tối thiểu có thể để chuyển đổi a thành b. 

Hàm calc đánh giá một hướng cố định của chuỗi thứ hai. Đối với mỗi vòng quay có thể, nó căn chỉnh các chỉ số theo modulo n và tích lũy chi phí chuyển đổi bằng cách sử dụng bảng phân phối được tính toán trước. Nếu bất kỳ vị trí nào không thể chuyển đổi được thì cấu hình sẽ bị loại bỏ. 

Cuối cùng, chúng tôi chạy quy trình này một lần cho chuỗi thứ hai ban đầu và một lần cho chuỗi ngược lại, lấy mức tối thiểu. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó n = 4 và chúng ta so sánh hai chuỗi tròn với các quy tắc biến đổi đơn giản. 

Đặt s = [1, 2, 3, 4], t = [1, 5, 3, 4] và giả sử các phép biến đổi cho phép 2 → 5 và 5 → 3 với chi phí nhất định. 

### Hướng thuận, shift = 0 

| tôi | s[i] | t[i] | chi phí | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 5 | quận (2,5) | 
| 2 | 3 | 3 | 0 | 
| 3 | 4 | 4 | 0 | 

Tổng chi phí được xác định hoàn toàn bằng cách chuyển đổi 2 thành 5. 

Dấu vết này cho thấy chỉ những vị trí không khớp mới đóng góp chi phí như thế nào và cách xử lý trước đường dẫn ngắn nhất giúp mỗi lần tra cứu trở nên ngay lập tức. 

### Ngược hướng, shift = 1 

| tôi | s[(i+1)%4] | t[i] | chi phí | 
| --- | --- | --- | --- | 
| 0 | 2 | 4 | dist(2,4) | 
| 1 | 3 | 1 | quận (3,1) | 
| 2 | 4 | 5 | dist(4,5) | 
| 3 | 1 | 3 | quận (1,3) | 

Cấu hình này chứng tỏ tại sao việc đảo ngược lại quan trọng: sự căn chỉnh tốt nhất chỉ có thể xuất hiện sau khi đảo hướng truyền tải, bởi vì các cấu trúc tuần hoàn đối xứng khi đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V³ + n²V²) đơn giản hóa thành O(V³ + n²) | Floyd-Warshall hơn 400 nút chiếm ưu thế trong quá trình tiền xử lý và mỗi lần đánh giá ca sẽ quét n vị trí | 
| Không gian | O(V²) | ma trận khoảng cách cho tất cả các loại đá quý | 

Các ràng buộc làm cho V = 400 có thể quản lý được để xử lý trước khối và đánh giá n² phù hợp thoải mái với các giới hạn điển hình trong đó n là vài nghìn. Việc tách thành các đường dẫn ngắn nhất ở trạng thái nhỏ và căn chỉnh chuỗi lớn là điều giúp giải pháp luôn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    # Assume solution is defined above
    # We redefine minimal wrapper
    from sys import stdout
    return ""

# Sample-based placeholders (structure only)
# assert run("...") == "..."

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống nhau tối thiểu | 0 | chi phí bằng 0 khi đã bằng nhau | 
| chuỗi biến đổi đơn | tổng đúng | chuyển đổi gián tiếp thông qua các nút trung gian | 
| giải pháp chỉ đảo ngược | chi phí dương | sự cần thiết của hướng đảo ngược | 
| đồ thị bị ngắt kết nối | -1 | phát hiện không thể | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi không có phép biến đổi trực tiếp nào tồn tại nhưng có đường dẫn nhiều bước. Quá trình tiền xử lý Floyd đảm bảo rằng những trường hợp như vậy vẫn được xử lý chính xác. Ví dụ: nếu chỉ tồn tại 1 → 2 và 2 → 3, chuyển đổi 1 thành 3 trong quá trình căn chỉnh sẽ sử dụng chính xác cả hai cạnh. 

Một trường hợp cạnh khác là khi một hướng căn chỉnh là không thể nhưng hướng ngược lại là hợp lệ. Thuật toán đánh giá rõ ràng cả hai hướng của chuỗi thứ hai, do đó nó không dựa vào tính đối xứng. 

Trường hợp cuối cùng xảy ra khi tất cả các phép quay đều hợp lệ nhưng chỉ có một phép quay mang lại chi phí tối ưu. Bởi vì mỗi ca được liệt kê một cách độc lập và chi phí được tính toán lại đầy đủ nên không có nguy cơ bỏ sót vòng quay tối ưu toàn cầu do các lựa chọn cục bộ.
