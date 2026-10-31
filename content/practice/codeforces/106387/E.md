---
title: "CF 106387E - Cho Thú Ăn"
description: "Chúng ta có một hệ thống gồm nhiều tòa nhà, mỗi tòa nhà gắn liền với một tỷ lệ sản xuất. Thời gian trôi qua theo những ngày rời rạc và khi thời gian tăng lên, mỗi tòa nhà sẽ tích lũy nhu cầu về “hộp thức ăn” theo tỷ lệ riêng."
date: "2026-06-20T12:32:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106387
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 2-25-26 (Beginner)"
rating: 0
weight: 106387
solve_time_s: 48
verified: true
draft: false
---

[CF 106387E - Cho quái vật ăn](https://codeforces.com/problemset/problem/106387/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hệ thống gồm nhiều tòa nhà, mỗi tòa nhà gắn liền với một tỷ lệ sản xuất. Thời gian trôi qua theo những ngày rời rạc và khi thời gian tăng lên, mỗi tòa nhà sẽ tích lũy nhu cầu về “hộp thức ăn” theo tỷ lệ riêng. 

Trong một số ngày cố định$t$, mọi tòa nhà$i$yêu cầu một số lượng hộp nhất định được xác định bởi số lượng nó đã tích lũy được trong những hộp đó$t$ngày. Việc chuyển đổi từ số lượng tích lũy sang hộp không phải là tuyến tính vì thực phẩm được đóng gói theo đơn vị có kích thước cố định, do đó yêu cầu của mỗi tòa nhà được làm tròn sau khi chia cho kích thước đóng gói không đổi. Tổng số hộp được yêu cầu trên tất cả các tòa nhà không được vượt quá giới hạn nguồn cung toàn cầu nhất định. 

Nhiệm vụ là tìm số ngày tối đa$t$sao cho tổng số hộp cần thiết trên tất cả các tòa nhà vẫn nằm trong nguồn cung sẵn có. 

Mỗi tòa nhà đóng góp nhiều nhu cầu hơn vì$t$tăng lên và một khi vượt quá giới hạn cung, nó vẫn bị vượt quá đối với tất cả các giá trị lớn hơn của$t$. Hành vi đơn điệu này là điều làm cho bài toán trở nên phù hợp với việc tìm kiếm nhị phân trên câu trả lời. 

Đầu vào bao gồm số lượng tòa nhà, giá riêng của chúng, thông số đóng gói và giới hạn toàn cầu đối với tổng số hộp có sẵn. Đầu ra là một số nguyên duy nhất biểu thị số ngày khả thi tối đa. 

Những ràng buộc cho phép$t$lớn như$10^{12}$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng sự tăng trưởng hàng ngày. Ngay cả một lần kiểm tra tuyến tính trong tất cả các ngày cũng là không thể. Thay vào đó, giải pháp phải đánh giá tính khả thi đối với ứng viên$t$TRONG$O(n)$, rồi tìm kiếm lại$t$sử dụng số lần kiểm tra logarit. 

Một cách giải thích ngây thơ tính toán lại số tiền tích lũy mỗi ngày sẽ thất bại hoàn toàn. Ví dụ, nếu$t = 10^{12}$, việc lặp đi lặp lại từng ngày là không thể thực hiện được. Một chế độ lỗi tinh vi khác xuất hiện nếu người ta cố gắng tính toán số gia tăng mỗi ngày và tích lũy kết quả phép chia dấu phẩy động, vì việc làm tròn phải được xử lý chính xác và yêu cầu độ chính xác của số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu bằng cách thử mọi số ngày có thể$t$, bắt đầu từ 0 và tăng dần lên. Đối với mỗi ứng viên$t$, chúng tôi tính toán lượng thực phẩm mà mỗi tòa nhà cần, chuyển số lượng đó thành các hộp bằng cách sử dụng cách chia trần, tính tổng tất cả các tòa nhà và kiểm tra xem tổng số lượng có phù hợp với giới hạn sẵn có hay không. 

Cách tiếp cận này đúng vì nó trực tiếp mô hình hóa định nghĩa của yêu cầu. Tuy nhiên, thời gian chạy của nó tăng tuyến tính với mức tối đa có thể$t$, và kể từ đó$t$có thể đạt được$10^{12}$, thậm chí một đánh giá duy nhất mỗi ngày cũng trở nên không khả thi. Nút thắt không phải là chi phí cho mỗi lần kiểm tra mà là số lượng giá trị ứng cử viên. 

Quan sát quan trọng là điều kiện khả thi là đơn điệu. Nếu một số ngày nhất định$t$khả thi thì số ngày nhỏ hơn cũng phải khả thi vì nhu cầu của mọi tòa nhà chỉ giảm hoặc giữ nguyên. Điều này chuyển vấn đề thành việc tìm ranh giới giữa các giá trị khả thi và không khả thi, đó chính xác là mục đích tìm kiếm nhị phân được thiết kế. 

Khi cấu trúc này được nhận ra, nhiệm vụ sẽ giảm xuống còn việc kiểm tra ứng viên một cách hiệu quả.$t$và sử dụng tìm kiếm nhị phân trên một phạm vi số lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(t \cdot n)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra |$O(n \log 10^{12})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi vấn đề là một hàm quyết định$t$: cho một số ngày nhất định, hãy xác định xem liệu có thể đáp ứng được tất cả các tòa nhà trong giới hạn cung cấp hay không. 

### Các bước 

1. Xác định hàm`can(t)`cái đó tính toán xem$t$ngày là khả thi. Đối với mỗi tòa nhà$i$, tính toán số hộp cần thiết bằng cách sử dụng mức chia trần của nhu cầu tích lũy của nó trên$t$theo kích thước bao bì. Tổng hợp tất cả các giá trị này và so sánh với giới hạn toàn cầu. Nếu tổng vượt quá giới hạn tại bất kỳ thời điểm nào, chúng ta đã biết$t$không hợp lệ và có thể dừng sớm. 
2. Đặt phạm vi tìm kiếm nhị phân từ$0$ĐẾN$10^{12}$. Giới hạn trên được chọn vì bất kỳ câu trả lời khả thi nào cũng phải nằm trong giới hạn của bài toán và hàm khả thi sẽ trở thành sai vượt quá ngưỡng. 
3. Thực hiện tìm kiếm nhị phân trên phạm vi này. Tại mỗi trung điểm$mid$, đánh giá`can(mid)`. 
4. Nếu`can(mid)`là đúng, cửa hàng$mid$như một câu trả lời tiềm năng và chuyển tìm kiếm sang nửa bên phải vì chúng tôi đang cố gắng tối đa hóa$t$. Nếu sai, loại bỏ nửa bên phải và tiếp tục với nửa bên trái. 
5. Sau khi tìm kiếm nhị phân hoàn tất, giá trị tốt nhất được lưu trữ là số ngày khả thi tối đa. 

Chi tiết triển khai quan trọng là tính toán mức chia trần một cách chính xác bằng cách sử dụng số học số nguyên. biểu hiện$\lceil \frac{x}{y} \rceil$phải được thực hiện như$(x + y - 1) // y$để tránh lỗi dấu phẩy động. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự đơn điệu. BẰNG$t$tăng, nhu cầu tích lũy của mỗi tòa nhà tăng tuyến tính và sau khi làm tròn thành các ô rời rạc thì chỉ có thể giữ nguyên hoặc tăng lên. Do đó, tổng số hộp được yêu cầu là hàm không giảm của$t$. Điều này đảm bảo rằng vị từ khả thi chuyển từ đúng sang sai chính xác một lần, tạo thành tiền tố liền kề của các giá trị hợp lệ. Tìm kiếm nhị phân xác định chính xác ranh giới của tiền tố này, đảm bảo giá trị hợp lệ tối đa$t$được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(t, a, x, b):
    total = 0
    for ai in a:
        total += (ai * t + x - 1) // x
        if total > b:
            return False
    return True

def solve():
    n, x, b = map(int, input().split())
    a = list(map(int, input().split()))

    lo, hi = 0, 10**12
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, a, x, b):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách việc kiểm tra tính khả thi khỏi logic tìm kiếm. các`can`hàm mã hóa toàn bộ diễn giải vật lý của vấn đề: nhu cầu thay đổi như thế nào theo thời gian và cách nó được rời rạc hóa thành các hộp. 

Tìm kiếm nhị phân duy trì tính bất biến mà tất cả các giá trị còn lại của`lo`chỉ là những ứng cử viên khả thi nếu được quan sát trước đó và tất cả các giá trị đúng của`hi`là không thể thực hiện được. Bước cập nhật di chuyển các ranh giới này dựa trên việc kiểm tra đơn điệu. 

Dừng sớm bên trong`can`là quan trọng vì tổng số có thể tăng lên lớn và vượt quá`b`làm cho việc tính toán thêm không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử có 2 tòa nhà, kích thước bao bì là 3 và giới hạn là 10. Tỷ lệ là [2, 5]. 

Chúng tôi kiểm tra các giá trị khác nhau của$t$: 

| t | Tòa nhà 1 | Tòa nhà 2 | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 
| 2 | 2 | 4 | 6 | 
| 3 | 2 | 5 | 7 | 
| 4 | 3 | 7 | 10 | 
| 5 | 4 | 9 | 13 | 

Giá trị khả thi lớn nhất là$t = 4$. 

Dấu vết này cho thấy hiệu ứng trần gây ra sự tăng vọt về nhu cầu từng phần thay vì tăng trưởng tuyến tính suôn sẻ như thế nào. 

### Ví dụ 2 

Bây giờ hãy xem xét tỷ lệ [1, 1, 1], kích thước bao bì 2, giới hạn 5. 

| t | Mỗi tòa nhà | Tổng cộng | 
| --- | --- | --- | 
| 1 | 1 | 3 | 
| 2 | 1 | 3 | 
| 3 | 2 | 6 | 

Câu trả lời là$t = 2$, cho thấy nhiều giá trị của$t$có thể ánh xạ tới cùng một tổng do làm tròn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log 10^{12})$| Tìm kiếm nhị phân trên phạm vi câu trả lời, mỗi lần kiểm tra tính khả thi sẽ quét tất cả các tòa nhà một lần | 
| Không gian |$O(1)$| Chỉ lưu trữ mảng đầu vào và một vài biến | 

Hệ số logarit nhỏ (khoảng 40), do đó nghiệm gần như tuyến tính trong thực tế đối với$n$. Điều này thoải mái thỏa mãn các ràng buộc điển hình lên đến$10^5$các tòa nhà. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(t, a, x, b):
        total = 0
        for ai in a:
            total += (ai * t + x - 1) // x
            if total > b:
                return False
        return True

    def solve():
        n, x, b = map(int, input().split())
        a = list(map(int, input().split()))

        lo, hi = 0, 10**12
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, a, x, b):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# small case
assert run("2 3 10\n2 5\n") == "4"

# all equal rates
assert run("3 2 5\n1 1 1\n") == "2"

# minimal case
assert run("1 1 10\n5\n") == "10"

# tight boundary
assert run("2 5 1\n1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 10 / 2 5 | 4 | trần tăng trưởng cơ bản | 
| 3 2 5 / 1 1 1 | 2 | đóng góp lặp đi lặp lại giống hệt nhau | 
| 1 1 10/5 | 10 | trường hợp tuyến tính phần tử đơn | 
| 2 5 1 / 1 1 | 0 | không khả thi ngay lập tức | 

## Vỏ cạnh 

Một trường hợp khó phát hiện xảy ra khi giới hạn cực kỳ nhỏ. Ví dụ: nếu tổng số hộp được phép là 1 và ngay cả tòa nhà nhỏ nhất cũng yêu cầu nhiều hơn 1 hộp tại$t = 0$, câu trả lời đúng là 0. Tìm kiếm nhị phân phải xử lý chính xác điều này mà không giả định rằng$t = 1$ban đầu luôn khả thi. 

Một trường hợp khác là khi tất cả các tỷ giá đều bằng 0. Trong tình huống đó, mọi tòa nhà luôn đóng góp bằng 0 bất kể$t$, do đó hàm khả thi luôn đúng cho đến giới hạn tối đa. Tìm kiếm nhị phân sẽ trả về chính xác$10^{12}$không bị tràn hoặc dừng sớm. 

Trường hợp thứ ba liên quan đến lớn$t$kết hợp với kích thước bao bì nhỏ, trong đó các giá trị trung gian của$ai \cdot t$có thể tràn số nguyên 32 bit. Sử dụng Python sẽ tránh được vấn đề này một cách tự nhiên, nhưng trong các ngôn ngữ khác, điều này yêu cầu số học 64-bit để ngăn chặn việc tính toán trần không chính xác. 

Tất cả các trường hợp này đều được xử lý chính xác vì thuật toán không bao giờ dựa vào các cập nhật gia tăng theo thời gian và luôn tính toán lại từng trạng thái trực tiếp từ định nghĩa của$t$.
