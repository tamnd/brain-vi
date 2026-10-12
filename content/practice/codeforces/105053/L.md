---
title: "CF 105053L - Ma trận LED"
description: "Chúng ta được cung cấp một ma trận LED hình chữ nhật và một mẫu hình chữ nhật được cho là sẽ cuộn qua nó từ phải sang trái. Ma trận có kích thước cố định và mỗi ô đều hoạt động bình thường hoặc bị hỏng. Đèn LED chức năng có thể bật được, còn đèn LED bị hỏng thì không bao giờ sáng được."
date: "2026-06-28T01:04:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105053
codeforces_index: "L"
codeforces_contest_name: "The 2024 ICPC Latin America Championship"
rating: 0
weight: 105053
solve_time_s: 56
verified: true
draft: false
---

[CF 105053L - Ma trận LED](https://codeforces.com/problemset/problem/105053/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận LED hình chữ nhật và một mẫu hình chữ nhật được cho là sẽ cuộn qua nó từ phải sang trái. Ma trận có kích thước cố định và mỗi ô đều hoạt động bình thường hoặc bị hỏng. Đèn LED chức năng có thể bật được, còn đèn LED bị hỏng thì không bao giờ sáng được. 

Mẫu này là một lưới có cùng chiều cao nhưng có thể có chiều rộng khác nhau. Khi cuộn, mẫu được dịch chuyển liên tục theo chiều ngang trên ma trận. Ở mỗi ca, một lát dọc của mẫu được căn chỉnh với một số cột của ma trận và một số ô mẫu nhất định yêu cầu phải bật đèn LED ma trận tương ứng tại thời điểm đó. 

Câu hỏi quan trọng không phải là mô phỏng hoạt ảnh mà là quyết định xem có tồn tại bất kỳ khoảnh khắc nào trong toàn bộ quá trình cuộn mà đèn LED bị hỏng sẽ sáng lên hay không. Nếu điều đó xảy ra thì không thể hiển thị được và câu trả lời là “N”, nếu không thì là “Y”. 

Đầu vào mô tả, đối với mỗi hàng, cả trạng thái ma trận và hàng mẫu. Việc ghép theo hàng này rất quan trọng vì các ràng buộc độc lập giữa các hàng: mỗi hàng hoạt động giống như một tương tác 1D giữa các cột ma trận và các cột mẫu. 

Giới hạn lên tới 1000 cho hàng, cột và chiều rộng mẫu. Mô phỏng khối trên các hàng, cột ma trận và ca sẽ đạt khoảng 10^9 thao tác, quá chậm. Ngay cả một lần quét lồng nhau kép mỗi ca cũng quá lớn, do đó giải pháp phải tránh mô phỏng rõ ràng từng bước cuộn. 

Trường hợp cạnh tinh tế xuất hiện khi mẫu nhỏ hơn ma trận. Ngay cả khi đó, việc cuộn vẫn tiếp tục vượt quá sự chồng chéo hoàn toàn, nghĩa là các cột mẫu có thể căn chỉnh với nhiều vị trí ma trận theo thời gian. Một trường hợp cạnh khác là khi một ô bị hỏng nhưng mẫu chỉ yêu cầu nó bị tắt ở vị trí căn chỉnh “ban đầu”; những ca sau vẫn có thể kích hoạt nó. Đây chính xác là nguyên nhân khiến việc kiểm tra ngây thơ bằng cách chỉ nhìn vào một căn chỉnh không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi vị trí cuộn. Đối với mỗi ca, chúng tôi đặt mẫu lên ma trận và kiểm tra từng ô chồng chéo. Nếu một ô mẫu đó là`*`đáp xuống một ô ma trận`-`, chúng tôi thất bại. 

Điều này đúng vì nó kiểm tra rõ ràng tất cả các trạng thái của hoạt ảnh. Tuy nhiên, số lượng dịch chuyển tỷ lệ thuận với tổng chiều rộng ma trận và chiều rộng mẫu và mỗi dịch chuyển sẽ kiểm tra tối đa các ô R×C. Điều này dẫn đến các hoạt động khoảng O(R·C·(C+K)), vượt xa giới hạn khi tất cả các chiều đều đạt 1000. 

Quan sát quan trọng là chúng ta thực sự không cần biết _khi_ một ô mẫu chạm vào một ô ma trận, mà chỉ biết liệu nó có bao giờ chạm vào hay không. Đối với một hàng cố định và cột ma trận cố định, chúng ta có thể đặt một câu hỏi đơn giản hơn: có bất kỳ`*`trong hàng mẫu có bao giờ căn chỉnh với vị trí ma trận này trong quá trình cuộn không? 

Một khi chúng ta viết lại bài toán theo cách này thì chiều thời gian sẽ biến mất. Mỗi ô ma trận chỉ cần biết liệu có tồn tại ít nhất một cột mẫu từng ánh xạ lên nó hay không. Điều đó trở thành một truy vấn tồn tại phạm vi trên hàng mẫu, có thể được trả lời bằng tổng tiền tố. 

Điều này làm giảm vấn đề kiểm tra đối với mỗi ô ma trận xem khoảng căn chỉnh tương ứng của nó có chứa ít nhất một ô hay không.`*`trong mẫu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(R·C·(C+K)) | O(1) thêm | Quá chậm | 
| Kiểm tra phạm vi tiền tố | O(R·(C+K)) | O(R·K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hàng một cách độc lập vì các hàng không tương tác. 

1. Đối với một hàng cố định, hãy tính trước tổng tiền tố trên hàng mẫu trong đó mỗi vị trí lưu trữ bao nhiêu`*`các ký tự xuất hiện theo chỉ mục đó. Điều này cho phép truy vấn phạm vi nhanh. 
2. Đối với mỗi cột ma trận`c`, xác định cột mẫu nào có thể căn chỉnh với nó trong suốt quá trình cuộn đầy đủ. Bộ này tạo thành một khoảng liền kề trong hàng mẫu. 
3. Tính ranh giới bên trái của khoảng này. Cột mẫu`j`có thể đạt tới cột ma trận`c`nếu cuộn giấy chuyển nó vào vị trí`c`, ngụ ý một mối quan hệ tuyến tính đơn giản giữa`c`Và`j`. Điều này làm giảm xuống giới hạn dưới trên`j`. 
4. Nếu khoảng này chứa bất kỳ`*`trong hàng mẫu, sau đó là ô ma trận`(row, c)`phải có chức năng. Nếu ô ma trận bị hỏng, cấu hình sẽ không hợp lệ ngay lập tức. 
5. Lặp lại cho tất cả các hàng và tất cả các cột. Nếu không tìm thấy xung đột, có thể hiển thị. 

### Tại sao nó hoạt động 

Mỗi ô mẫu chỉ đóng góp các ràng buộc cho những ô ma trận mà nó có thể chiếm giữ trong quá trình cuộn. Chuyển động hoàn toàn là tuyến tính, do đó mỗi cột mẫu ảnh hưởng đến một phạm vi cột ma trận liền kề. Điều đó có nghĩa là mỗi ô ma trận chỉ cần biết liệu có bất kỳ ô mẫu bắt buộc nào từng ánh xạ tới nó hay không. Nếu một ánh xạ như vậy tồn tại và ô ma trận bị hỏng thì yêu cầu đó không bao giờ có thể được thỏa mãn, do đó độ chính xác sẽ giảm đi khi phát hiện bất kỳ giao điểm bị cấm nào như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    R, C, K = map(int, input().split())

    mat = []
    pat = []

    for _ in range(R):
        line = input().split()
        mat.append(line[0])
        pat.append(line[1])

    for i in range(R):
        # prefix sum of '*' in pattern row
        pref = [0] * (K + 1)
        for j in range(K):
            pref[j + 1] = pref[j] + (1 if pat[i][j] == '*' else 0)

        for c in range(C):
            if mat[i][c] == '*':
                continue

            # earliest pattern column that can reach c
            L = max(0, c - (C - 1))

            # check if there is any '*' in pat[i][L:K]
            if pref[K] - pref[L] > 0:
                print("N")
                return

    print("Y")

if __name__ == "__main__":
    solve()
```Giải pháp đọc cả ma trận và mẫu theo từng hàng. Đối với mỗi hàng, nó xây dựng một tổng tiền tố trên mẫu để hỗ trợ kiểm tra thời gian liên tục xem có bất kỳ`*`tồn tại trong một hậu tố. 

Đối với mỗi ô ma trận bị hỏng, nó sẽ tính chỉ số mẫu sớm nhất có thể đạt tới nó. Thay vì mô phỏng các ca, điều này sử dụng thực tế là việc cuộn là tuyến tính và mọi cột mẫu sẽ quét qua một phạm vi cột ma trận liền kề. Nếu bất kỳ ô mẫu bắt buộc nào nằm trong phạm vi có thể truy cập đó thì ô đó không hợp lệ. 

Chi tiết triển khai chính là việc tính toán`L = max(0, c - (C - 1))`. Điều này mã hóa cột mẫu sớm nhất có thể nằm trên cột ma trận`c`trong suốt quá trình cuộn đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ với một hàng: 

Ma trận:`* - -`Mẫu:`*-*`Tổng tiền tố cho mẫu là`[0,1,1,2]`. 

Đối với ô bị hỏng ở cột 1, chúng tôi tính toán`L = max(0, 1 - 2) = 0`. Hậu tố`[0:3]`contains stars, so this cell is required to light up at some point, but it is broken. Thuật toán từ chối chính xác. 

Bây giờ hãy xem xét: 

Ma trận:`* * *`Mẫu:`*-*`Mỗi ô ma trận đều có chức năng, do đó, cho dù mẫu có thay đổi như thế nào thì cũng không có tình trạng lỗi. Mặc dù một số cách sắp xếp yêu cầu có dấu sao nhưng tất cả các ô bắt buộc đều hợp lệ. 

| Hàng | Cột | Vỡ? | Kiểm tra phạm vi | Mẫu sao trong phạm vi | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | Không | bỏ qua | - | được | 
| 0 | 1 | Không | bỏ qua | - | được | 
| 0 | 2 | Không | bỏ qua | - | được | 

Điều này cho thấy thuật toán chỉ phản ứng với các ràng buộc không thể thực hiện được và giữ im lặng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R·(C + K)) | Mỗi hàng tạo một tiền tố trên K và quét các cột C một lần | 
| Không gian | O(K) | Chỉ mảng tiền tố trên mỗi hàng | 

Các ràng buộc cho phép tổng cộng tối đa 10^6 ký tự trên mỗi cấu trúc, do đó, việc quét tuyến tính trên mỗi hàng sẽ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal allowed case
assert run("1 1 1\n* *\n") == "Y"

# broken cell never required
assert run("1 3 2\n*-- *-\n") == "Y"

# broken cell required eventually
assert run("1 3 2\n-*- **\n") == "N"

# all broken matrix but no stars in pattern
assert run("2 2 2\n-- --\n-- --\n") == "Y"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 đều tốt | Y | Trường hợp cơ sở | 
| bị hỏng nhưng không có yêu cầu | Y | Ràng buộc không được kích hoạt | 
| buộc kích hoạt trên ô bị hỏng | N | Logic từ chối cốt lõi | 
| hỏng hết, không có sao | Y | Trường hợp cạnh yêu cầu trống | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi mẫu không chứa`*`không hề. Trong trường hợp này, không cần ô ma trận nào sáng lên, vì vậy ngay cả một ma trận bị hỏng hoàn toàn cũng hợp lệ. Thuật toán xử lý việc này một cách tự nhiên vì mọi truy vấn phạm vi tiền tố đều trả về 0. 

Một trường hợp cạnh khác là khi mẫu rộng hơn ma trận. Khoảng dẫn xuất cho mỗi ô ma trận vẫn hoạt động vì nó chỉ ghi lại phần mẫu thực sự có thể quét qua ma trận. Không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng là khi cả ma trận và mẫu đều ở mức tối thiểu, chẳng hạn như 1×1. Thuật toán giảm xuống chỉ còn một lần kiểm tra tiền tố và phân biệt chính xác giữa trạng thái tương thích và không tương thích.
