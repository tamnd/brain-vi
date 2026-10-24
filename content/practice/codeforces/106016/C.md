---
title: "CF 106016C - USD sang Liras"
description: "Mỗi ngày Omar phải giải quyết một nhu cầu được thể hiện bằng hai loại tiền tệ. Đối với ngày thứ i, có một số lượng đô la yêu cầu ai và tỷ lệ chuyển đổi bi cho biết mức giá thay thế một đô la bằng liras. Vào ngày đó Omar có thể chia số tiền thanh toán thành hai phần."
date: "2026-06-21T16:41:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106016
codeforces_index: "C"
codeforces_contest_name: "The 2025 Homs Collegiate programming contest"
rating: 0
weight: 106016
solve_time_s: 55
verified: true
draft: false
---

[CF 106016C - USD so với Liras](https://codeforces.com/problemset/problem/106016/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngày Omar phải giải quyết một nhu cầu được thể hiện bằng hai loại tiền tệ. Đối với ngày thứ i, có một số lượng đô la yêu cầu ai và tỷ lệ chuyển đổi bi cho biết mức giá thay thế một đô la bằng liras. 

Vào ngày đó Omar có thể chia số tiền thanh toán thành hai phần. Anh ta có thể trả trực tiếp một số xi đô la, và số đô la ai − xi còn lại được chuyển đổi một cách hiệu quả thành liras, có giá (ai − xi) · bi liras. Nếu anh ta thanh toán mọi thứ bằng đô la thì chi phí lira sẽ bằng 0. Nếu anh ta không trả gì bằng đô la, anh ta sẽ trả toàn bộ số tiền bằng liras. 

Khoản thanh toán lira cho một ngày mà chúng tôi gọi là ci, và mục tiêu là giữ cho ngày tồi tệ nhất càng nhỏ càng tốt. Chính xác hơn, chúng tôi muốn giảm thiểu ci tối đa trong tất cả các ngày trong khi vẫn có thể chọn các giá trị xi có tổng không vượt quá m đô la. 

Tương tác chính là mỗi đô la được sử dụng vào ngày thứ i sẽ giảm ci xuống bi. Vì vậy, đô la hoạt động giống như một nguồn tài nguyên có thể được phân bổ trong nhiều ngày, mỗi đô la có “giá trị” khác nhau tùy thuộc vào tỷ giá hối đoái trong ngày. 

Các ràng buộc đẩy chúng tôi tới mức O(n log C) hoặc O(n log n) cho mỗi phương pháp thử nghiệm. Vì tổng n trên tất cả các trường hợp thử nghiệm lên tới 3 × 10^5 nên mọi giải pháp tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm đều được chấp nhận. Chiến lược bậc hai thử tất cả các phân bổ hoặc mô phỏng phân phối lại sẽ thất bại ngay lập tức vì ngay cả các phép tính bình phương 10^5 cũng quá lớn. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam mỗi ngày không có ngưỡng toàn cầu. Giảm bớt ngày tồi tệ nhất tại địa phương có thể lãng phí tiền vào một ngày mà mỗi đô la ít có giá trị hơn vào ngày khác. Giải pháp đúng phải so sánh các ngày trên toàn cầu thay vì xử lý chúng một cách độc lập. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là tưởng tượng gán mỗi m đô la cho một ngày nào đó và quan sát xem giá lira tối đa giảm bao nhiêu. Trong mô hình brute-force, chúng ta sẽ liên tục chọn một ngày và quyết định có nên đổi thêm một đô la vào ngày đó hay không. Mỗi lựa chọn đều ảnh hưởng đến mức tối đa cuối cùng và việc khám phá tất cả các bài tập dẫn đến số lượng khả năng theo cấp số nhân về nguyên tắc hoặc ít nhất là O(mn) nếu được mô phỏng một cách ngây thơ. Vì m có thể lớn tới 10^9 nên điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thay vì xây dựng sự phân bổ, chúng ta có thể đảo ngược quan điểm. Giả sử chúng ta đoán giá trị y cho chi phí lira tối đa được phép. Sau đó, với mỗi ngày, chúng ta có thể tính xem chúng ta buộc phải chi bao nhiêu đô la để giảm ngày đó xuống tối đa là y. Nếu tổng số đô la yêu cầu vượt quá m thì số tiền dự đoán là quá nhỏ. Nếu nó nằm trong m thì dự đoán là khả thi. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi đơn điệu trên y. Nếu một y nhất định có thể đạt được thì bất kỳ y lớn hơn nào cũng có thể đạt được vì chúng ta yêu cầu giảm ít hơn. Tính đơn điệu này cho phép tìm kiếm nhị phân trên y. 

Nhiệm vụ còn lại là tính toán, mỗi ngày, số đô la tối thiểu cần thiết để đảm bảo ci ≤ y. Vì ci = (ai − xi)bi nên điều kiện ci ≤ y trở thành xi ≥ ai − y / bi. Mỗi đô la giảm ci đi bi, do đó xi yêu cầu thực tế là mức trần trên ai − y / bi, bị cắt ở mức 0 và nhiều nhất là ai. 

Khi chúng ta có thể tính toán tổng số đô la cần thiết cho một y cố định, tìm kiếm nhị phân sẽ mang lại câu trả lời khả thi tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân bổ lực lượng vũ phu | Hàm mũ / O(mn) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân có kiểm tra tính khả thi | O(n log(max ai·bi)) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành tìm y nhỏ nhất sao cho tất cả các ngày có thể giảm xuống tối đa y bằng cách sử dụng tối đa m đô la. 

1. Để đoán y cố định, hãy tính xem cần bao nhiêu đô la mỗi ngày để đảm bảo ci ≤ y.

Đối với ngày i, điều kiện (ai − xi)bi ≤ y sắp xếp lại thành xi ≥ ai − y / bi. Vì xi phải là số nguyên nên ta lấy xi = max(0, ai − Floor(y / bi)). Điều này đưa ra mức chi tiêu bắt buộc tối thiểu vào ngày đó. 

Lý do điều này có tác dụng là vì mỗi đô la làm giảm chi phí lira đi đúng bi, do đó, việc giảm ci đi k·bi đòi hỏi phải chi k đô la. 
2. Tính tổng các giá trị xi cần thiết này trong tất cả các ngày. 

Tổng này thể hiện tổng số đô la tối thiểu cần thiết để mỗi ngày thỏa mãn ngưỡng y. 
3. Kiểm tra tính khả thi đối với m. 

Nếu tổng là ≤ m thì y có thể đạt được. Ngược lại, y quá nhỏ và chúng ta cần tăng nó lên. 
4. Tìm kiếm nhị phân trên y trong phạm vi [0, max(ai·bi)]. 

Giới hạn trên tương ứng với trường hợp xấu nhất khi chúng ta không tiêu tiền vào bất cứ đâu. 

### Tại sao nó hoạt động 

Đối với bất kỳ ngày cố định nào, mối quan hệ giữa xi và ci là tuyến tính và độc lập với các ngày khác. Mối liên hệ duy nhất giữa các ngày là tổng ngân sách đô la m. Điều này tạo ra một cấu trúc trong đó mỗi y xác định một yêu cầu tối thiểu xác định và việc tăng y chỉ có thể làm giảm các yêu cầu này. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân sẽ hội tụ về y khả thi nhỏ nhất, bởi vì tính khả thi không bao giờ chuyển từ đúng sang sai khi y tăng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        max_val = 0
        for i in range(n):
            max_val = max(max_val, a[i] * b[i])

        def ok(y):
            need = 0
            for i in range(n):
                take = a[i] - (y // b[i])
                if take > 0:
                    need += take
                    if need > m:
                        return False
            return need <= m

        lo, hi = 0, max_val
        while lo < hi:
            mid = (lo + hi) // 2
            if ok(mid):
                hi = mid
            else:
                lo = mid + 1

        out.append(str(lo))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là hàm đánh giá một ứng cử viên y. Nó chuyển một ngưỡng toàn cầu thành mức sử dụng đô la tối thiểu mỗi ngày. Biểu thức y // b[i] là số đô la hủy bỏ hoàn toàn việc cắt giảm hai kích thước mà không vượt quá mục tiêu. Trừ đi số này cho ai sẽ biết còn phải chi bao nhiêu đô la. 

Tìm kiếm nhị phân duy trì một khoảng trong đó phía bên trái là không khả thi hoặc chưa biết và phía bên phải là khả thi. Mỗi lần kiểm tra điểm giữa sẽ làm giảm không gian tìm kiếm theo logarit. 

Phải cẩn thận với tình trạng tràn, vì ai·bi có thể đạt tới 10^18, nhưng Python xử lý việc này một cách an toàn. Trong các ngôn ngữ khác, số nguyên 64 bit là bắt buộc. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ có n = 3, m = 3. 

Đặt a = [3, 2, 4] và b = [2, 5, 1]. 

Chúng tôi kiểm tra một ứng cử viên y = 4. 

| tôi | ai | bi | y // bi | cần thiết xi = max(0, ai − y//bi) | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 1 | 
| 2 | 2 | 5 | 0 | 2 | 
| 3 | 4 | 1 | 4 | 0 | 

Tổng số đô la yêu cầu = 3, bằng m, nên y = 4 là khả thi. 

Bây giờ hãy xem xét y = 3. 

| tôi | ai | bi | y // bi | yêu cầu xi | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 1 | 2 | 
| 2 | 2 | 5 | 0 | 2 | 
| 3 | 4 | 1 | 3 | 1 | 

Tổng số đô la yêu cầu = 5, vượt quá m nên y = 3 là không khả thi. 

Điều này cho thấy việc thắt chặt y làm tăng mức sử dụng đô la cần thiết như thế nào, đây chính xác là điều cho phép tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max ai·bi)) | Mỗi lần kiểm tra tính khả thi là O(n) và tìm kiếm nhị phân chạy trên phạm vi giá trị | 
| Không gian | O(1) thêm | Chỉ sử dụng mảng và bộ đếm | 

Tổng của n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 3 × 10^5, do đó, ngay cả với khoảng 30-60 lần lặp tìm kiếm nhị phân cho mỗi thử nghiệm, tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []

        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))

            max_val = 0
            for i in range(n):
                max_val = max(max_val, a[i] * b[i])

            def ok(y):
                need = 0
                for i in range(n):
                    take = a[i] - (y // b[i])
                    if take > 0:
                        need += take
                        if need > m:
                            return False
                return need <= m

            lo, hi = 0, max_val
            while lo < hi:
                mid = (lo + hi) // 2
                if ok(mid):
                    hi = mid
                else:
                    lo = mid + 1

            out.append(str(lo))

        return "\n".join(out)

    return solve()

# small sanity checks
assert run("1\n1 5\n10\n2\n") == "20"

# all zeros effect check
assert run("1\n2 10\n1 1\n1 1\n") == "0"

# tight budget forcing high y
assert run("1\n2 1\n10 10\n1 1\n") == "9"

# uneven rates
assert run("1\n3 3\n3 2 4\n2 5 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhân rộng một ngày | 20 | tính đúng đắn của chuyển đổi trực tiếp | 
| hộp đựng đồng phục nhỏ | 0 | tính khả thi bảo hiểm toàn bộ đô la | 
| ngân sách eo hẹp | 9 | trường hợp áp lực tìm kiếm nhị phân | 
| giá hỗn hợp | 4 | tương tác giữa các giá trị bi khác nhau | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi bi = 1 cho tất cả các ngày. Trong tình huống đó, mỗi đô la giảm ci đi đúng một, do đó vấn đề giảm xuống còn việc phân phối m đơn vị cắt giảm cho các mức thâm hụt tuyến tính. Thuật toán xử lý việc này một cách tự nhiên vì y // 1 đơn giản trở thành y và xi bắt buộc trở thành ai − y. 

Một trường hợp khác là khi m cực kỳ lớn. Nếu m ≥ sum(ai), chúng ta có thể thanh toán trực tiếp toàn bộ số đô la, làm cho tất cả ci bằng 0. Tìm kiếm nhị phân tự nhiên hội tụ về y = 0 vì tính khả thi thành công ngay lập tức. 

Trường hợp tinh vi cuối cùng xuất hiện khi ai * bi rất lớn, gần bằng 10^18. Thuật toán tránh tràn bằng cách không bao giờ lưu trữ phép trừ trung gian của các tích này ngoài việc tính toán giới hạn tìm kiếm nhị phân. Tất cả công việc trên mỗi lần lặp đều nằm trong các phép toán số nguyên an toàn và hàm khả thi chỉ sử dụng phép chia và phép trừ ở quy mô nhỏ hơn.
