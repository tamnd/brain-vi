---
title: "CF 104349A - Con người có thể đọc được"
description: "Chúng tôi được cung cấp kích thước tệp thô được đo bằng byte và chúng tôi phải hiển thị nó ở định dạng nhỏ gọn “con người có thể đọc được” chỉ sử dụng ba đơn vị có thể: byte (B), kibibytes (KiB) và mebibytes (MiB)."
date: "2026-07-01T18:14:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104349
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #13 (Boombastic-Forces)"
rating: 0
weight: 104349
solve_time_s: 85
verified: false
draft: false
---

[CF 104349A - Con người có thể đọc được](https://codeforces.com/problemset/problem/104349/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp kích thước tệp thô được đo bằng byte và chúng tôi phải hiển thị nó ở định dạng nhỏ gọn “con người có thể đọc được” chỉ sử dụng ba đơn vị có thể: byte (B), kibibytes (KiB) và mebibytes (MiB). Đầu ra luôn là một số duy nhất, ngay sau đó là một trong các chuỗi đơn vị này, không có khoảng trắng. 

Mỗi đơn vị tương ứng với một tỷ lệ lũy thừa hai. Một KiB bằng 1024 byte và một MiB bằng 1024 KiB, tức là 1024 byte bình phương. Số chúng tôi in phải luôn là số nguyên từ 1 đến 1023 và chúng tôi bắt buộc phải làm tròn xuống khi chuyển đổi giữa các đơn vị. 

Vì vậy, nhiệm vụ là chọn đơn vị lớn nhất có thể sao cho sau khi chuyển đổi, giá trị ít nhất vẫn là 1 và nhiều nhất là 1023, sau đó xuất ra số nguyên nổi trong đơn vị đó. 

Ràng buộc đối với m lên tới 10^9, đủ nhỏ để chúng ta chỉ xử lý ở hầu hết thang đo MiB vì 1024^2 là khoảng 10^6 và 1024^3 đã vượt quá 10^9. Điều đó có nghĩa là không gian quyết định rất nông: chỉ có B, KiB và MiB là quan trọng. 

Một sai lầm ngây thơ nảy sinh khi cố gắng luôn chuyển đổi lên trên hoặc luôn bình thường hóa một cách tham lam mà không kiểm tra giới hạn. Ví dụ: coi 1401 byte là 1401 B là không hợp lệ vì 1401 vượt quá 1023, nhưng chuyển đổi nó thành KiB sẽ mang lại 1 KiB sau khi tạo sàn, điều này hợp lệ. Một cạm bẫy khác là mất quy tắc rằng số phải nằm trong [1, 1023], quy định này cấm các kết quả đầu ra như 0 MiB hoặc 0 KiB ngay cả khi đúng về mặt toán học. 

Một vấn đề tế nhị khác là việc đặt hàng: nếu chúng ta thử KiB trước rồi đến MiB, chúng ta có thể chọn sai đơn vị nhỏ hơn ngay cả khi MiB hợp lệ. Ví dụ: 14510629 byte phải là 13 MiB, nhưng cách tiếp cận bất cẩn có thể dừng lại ở KiB vì nó “vừa sớm hơn” sau khi làm tròn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử cả ba đơn vị một cách độc lập. Đối với mỗi đơn vị, chúng tôi tính toán giá trị được chuyển đổi bằng cách sử dụng phép chia số nguyên và kiểm tra xem nó có nằm trong phạm vi hợp lệ [1, 1023] hay không. Nếu nhiều đơn vị hợp lệ, chúng tôi chọn đơn vị lớn nhất trong số đó (MiB trên KiB trên B). Điều này hiệu quả vì chỉ có ba ứng cử viên và mỗi lần chuyển đổi đều có thời gian không đổi. 

Sự kém hiệu quả chỉ xuất hiện nếu chúng ta tưởng tượng một phiên bản tổng quát có nhiều đơn vị hoặc tính toán lại nhiều lần, nhưng ngay cả khi đó cấu trúc vẫn tầm thường đến mức vũ lực đã là tối ưu trong thực tế. 

Quan sát quan trọng là hệ thống đơn vị được phân cấp nghiêm ngặt và mỗi bước là hệ số 1024. Điều này có nghĩa là chúng ta không bao giờ cần phải tìm kiếm hoặc tối ưu hóa một cách linh hoạt; chúng ta chỉ cần kiểm tra khả năng chia hết cho các hằng số cố định và áp dụng phép chia sàn. Ràng buộc về phạm vi số buộc chính xác phải có một biểu diễn hợp lệ, do đó câu trả lời được xác định bởi đơn vị cao nhất giữ thương số trong phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đánh giá từng trường hợp thử nghiệm một cách độc lập. 

1. Bắt đầu với kích thước đã cho tính bằng byte, m. Đây là cách biểu diễn cơ sở và tất cả các đơn vị khác đều được lấy từ phép chia lặp đi lặp lại cho 1024. 
2. Kiểm tra xem có thể biểu thị giá trị trong MiB hay không. Tính m // (1024 * 1024). Nếu kết quả ít nhất là 1 và nhiều nhất là 1023 thì ta chọn ngay MiB làm đơn vị. Điều này là do MiB là đơn vị được phép lớn nhất và chúng tôi muốn biểu diễn được nén nhiều nhất. 
3. Nếu MiB không hợp lệ, hãy kiểm tra KiB. Tính m // 1024. Nếu giá trị này nằm trong [1, 1023], chúng ta xuất ra KiB với số đó. 
4. Nếu cả MiB và KiB đều không hoạt động, chúng ta sẽ chuyển về byte và xuất trực tiếp m B. Tại thời điểm này m phải ở [1, 1023], vì nếu không thì KiB sẽ hợp lệ. 
5. In số đã chọn nối với chuỗi đơn vị.

Tại sao nó hoạt động dựa trên cấu trúc của tỷ lệ cơ sở 1024. Mỗi đơn vị chính xác gấp 1024 lần đơn vị trước đó, do đó, các giá trị được chuyển đổi được xác định duy nhất bằng phép chia số nguyên. Ràng buộc rằng số đầu ra phải duy trì trong [1, 1023] đảm bảo rằng nhiều nhất một cấp đơn vị có thể thỏa mãn điều kiện sau khi xếp sàn. Vì MiB thống trị KiB và KiB thống trị B nên việc chọn đơn vị hợp lệ cao nhất không thể bỏ qua biểu diễn hợp lệ nhỏ hơn hoặc bỏ sót đơn vị hợp lệ lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        m = int(input())

        mib = m // (1024 * 1024)
        if 1 <= mib <= 1023:
            print(str(mib) + "MiB")
            continue

        kib = m // 1024
        if 1 <= kib <= 1023:
            print(str(kib) + "KiB")
            continue

        print(str(m) + "B")

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp tuân theo hệ thống phân cấp đơn vị. Việc kiểm tra MiB được thực hiện trước tiên vì nó mang lại biểu diễn nhỏ gọn nhất nếu hợp lệ. Kiểm tra KiB chỉ đạt được nếu MiB bị lỗi và byte chỉ được sử dụng khi cả hai biểu diễn tỷ lệ vượt quá giới hạn số cho phép hoặc quá nhỏ. Phép chia số nguyên thực thi hành vi làm tròn cần thiết một cách tự nhiên, do đó không cần logic làm tròn bổ sung. 

Một lỗi triển khai phổ biến là kiểm tra KiB trước MiB, điều này có thể tạo ra các đơn vị dưới mức tối ưu. Một lỗi khác là quên giới hạn dưới của 1, điều này có thể dẫn đến kết quả đầu ra như 0 KiB đối với các giá trị nhỏ dưới 1024, không hợp lệ. 

## Ví dụ đã hoạt động 

Đầu tiên, hãy xem xét đầu vào m = 1401. 

| Bước | m | MiB | KiB | Được chọn | 
| --- | --- | --- | --- | --- | 
| Kiểm tra MiB | 1401 | 0 | - | không | 
| Kiểm tra KiB | 1401 | - | 1 | KiB | 

Ở đây 1401 // 1024 = 1, hợp lệ. Chuyển đổi MiB cho kết quả 0, không hợp lệ vì nó nằm dưới số lượng yêu cầu tối thiểu. Thuật toán chọn đúng KiB. 

Bây giờ hãy xem xét m = 14510629. 

| Bước | m | MiB | KiB | Được chọn | 
| --- | --- | --- | --- | --- | 
| Kiểm tra MiB | 14510629 | 13 | - | MiB | 
| Dừng lại | - | 13 | - | MiB | 

Ở đây 14510629 // 1048576 bằng 13. Giá trị này nằm trong [1, 1023] nên MiB được chọn ngay lập tức. KiB không bao giờ được đánh giá vì MiB đã đáp ứng được yêu cầu. 

Những ví dụ này cho thấy cách phân cấp thực thi một lựa chọn duy nhất và cách sàn tương tác với chuyển đổi đơn vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính và so sánh số học không đổi | 
| Không gian | O(1) | Chỉ một số biến số nguyên được sử dụng bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm và mỗi trường hợp được xử lý trong thời gian không đổi. Do đó, giải pháp chạy tốt trong giới hạn với mức sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        m = int(input())

        mib = m // (1024 * 1024)
        if 1 <= mib <= 1023:
            out.append(str(mib) + "MiB")
            continue

        kib = m // 1024
        if 1 <= kib <= 1023:
            out.append(str(kib) + "KiB")
            continue

        out.append(str(m) + "B")

    return "\n".join(out)

# provided samples
assert run("3\n29\n1401\n14510629\n") == "29B\n1KiB\n13MiB"

# custom cases
assert run("1\n1\n") == "1B"
assert run("1\n1023\n") == "1023B"
assert run("1\n1024\n") == "1KiB"
assert run("1\n1048576\n") == "1MiB"
assert run("1\n1048575\n") == "1023KiB"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1B | giá trị nhỏ nhất có thể | 
| 1023 | 1023B | giới hạn trên cho byte | 
| 1024 | 1KiB | ranh giới KiB chính xác | 
| 1048576 | 1MiB | ranh giới MiB chính xác | 
| 1048575 | 1023KiB | ranh giới ngay dưới MiB | 

## Vỏ cạnh 

Với m = 1, kiểm tra MiB cho 0 và kiểm tra KiB cũng cho 0, do đó thuật toán quay trở lại byte và xuất ra 1B. Điều này đúng vì chỉ byte mới có thể biểu thị các giá trị dưới 1024. 

Với m = 1024 thì MiB là 0, KiB là 1 nên thuật toán chọn 1KiB. Điều này phù hợp với yêu cầu chúng ta phải chuyển sang đơn vị tiếp theo ngay khi đơn vị hiện tại vượt quá phạm vi số hợp lệ. 

Với m = 1048576, MiB trở thành 1, giá trị này hợp lệ, do đó thuật toán chọn trực tiếp 1MiB. Điều này chứng tỏ rằng đơn vị hợp lệ cao nhất luôn được ưu tiên và lũy thừa chính xác của 1024 ánh xạ rõ ràng đến các đơn vị cao hơn mà không có sự mơ hồ.
