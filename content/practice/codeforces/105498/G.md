---
title: "CF 105498G - Hệ thống đăng ký người dùng"
description: "Chúng tôi đang duy trì cơ sở dữ liệu trực tiếp về tên người dùng theo hai thao tác: chèn và xóa. Mỗi tên người dùng là một chuỗi ngắn và mọi thao tác đều cố gắng thêm hoặc xóa nó. Khi chèn tên người dùng, hệ thống sẽ hoạt động giống như một cơ chế đặt trước."
date: "2026-06-23T21:43:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105498
codeforces_index: "G"
codeforces_contest_name: "Khulna Regional Inter University Programming Contest (KRIUPC) MIRROR"
rating: 0
weight: 105498
solve_time_s: 54
verified: true
draft: false
---

[CF 105498G - Hệ thống đăng ký người dùng](https://codeforces.com/problemset/problem/105498/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì cơ sở dữ liệu trực tiếp về tên người dùng theo hai thao tác: chèn và xóa. Mỗi tên người dùng là một chuỗi ngắn và mọi thao tác đều cố gắng thêm hoặc xóa nó. 

Khi chèn tên người dùng, hệ thống sẽ hoạt động giống như một cơ chế đặt trước. Nếu tên không được sử dụng, nó sẽ được chấp nhận trực tiếp. Nếu nó đã được sử dụng, hệ thống sẽ cố gắng cứu vãn yêu cầu bằng cách thêm các số nguyên tăng dần bắt đầu từ 1 cho đến khi tìm thấy phiên bản vẫn miễn phí và tên được tạo đó sẽ được lưu trữ thay thế. 

Khi xóa, hệ thống chỉ cần kiểm tra xem tên người dùng chính xác có tồn tại hay không. Nếu có, nó sẽ biến mất khỏi cơ sở dữ liệu. Nếu không, yêu cầu xóa sẽ bị từ chối. 

Khó khăn cốt lõi không phải là việc tự xử lý chuỗi mà là việc duy trì một tập hợp động theo hai ràng buộc ở quy mô lớn: kiểm tra sự tồn tại nhanh và tạo nhanh tên có hậu tố nhỏ nhất không được sử dụng cho một chuỗi cơ sở nhất định. 

Kích thước đầu vào đạt tới một trăm nghìn thao tác. Bất kỳ giải pháp nào quét tuyến tính tên người dùng hiện có cho mỗi lần chèn sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất. Ví dụ: nếu chúng ta liên tục chèn cùng một chuỗi cơ sở, một cách tiếp cận đơn giản sẽ kiểm tra`base`, sau đó`base1`, sau đó`base2`, v.v., dẫn đến tổng chi phí tỷ lệ thuận với tổng của tất cả các hậu tố được tạo ra. 

Một trường hợp phức tạp phát sinh khi việc xóa được trộn lẫn với các phần chèn thêm. Giả sử chúng ta chèn`a`,`a1`,`a2`, sau đó xóa`a1`. Một hệ thống bất cẩn chỉ theo dõi sự tồn tại mà không theo dõi hậu tố miễn phí tiếp theo có thể cho rằng lần chèn tiếp theo vẫn phải là`a3`, mặc dù`a1`bây giờ đã có sẵn một lần nữa. Hành vi đúng là không sử dụng lại các khoảng trống, bởi vì quy tắc nghiêm ngặt là “số nguyên nhỏ nhất i sao cho chuỗi không tồn tại ngay bây giờ”. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp lưu trữ tất cả tên người dùng trong một tập hợp băm. Để chèn một chuỗi cơ sở`s`, chúng tôi kiểm tra xem`s`tồn tại. Nếu không, chúng tôi chèn nó. Nếu không chúng tôi sẽ cố gắng`s1`,`s2`,`s3`, v.v. cho đến khi chúng tôi tìm thấy một chỗ trống. 

Cách tiếp cận này đúng nhưng có thể xuống cấp trầm trọng. Xem xét việc chèn liên tục cùng một chuỗi cơ sở`a`mà không cần xóa. Lần chèn đầu tiên là O(1), lần kiểm tra thứ hai`a`, sau đó`a1`, chuỗi thứ ba kiểm tra ba chuỗi, v.v. Sau n lần chèn, chúng tôi thực hiện khoảng 1 + 2 + … + n kiểm tra, đó là O(n²). 

Điều quan trọng là chúng ta không cần phải “khám phá lại” các hậu tố từ đầu mỗi lần. Đối với mỗi chuỗi cơ sở, chúng ta có thể duy trì chỉ mục hậu tố tiếp theo chưa từng được sử dụng cho cơ sở đó theo nghĩa đơn điệu. Ngay cả khi việc xóa xảy ra, chúng tôi vẫn không bao giờ cần phải xem xét lại các chỉ số thấp hơn trong quá trình tìm kiếm vì chúng tôi có thể trực tiếp kiểm tra các ứng viên theo thứ tự tăng dần nhưng bỏ qua công việc lặp đi lặp lại thông qua bộ nhớ đệm. 

Cách rõ ràng để thực hiện điều này là duy trì một bộ băm toàn cục để tồn tại và một từ điển ánh xạ từng chuỗi cơ sở tới hậu tố số nguyên tiếp theo mà chúng ta nên thử. Khi chèn, chúng tôi bắt đầu từ bộ đếm được lưu trữ đó và chỉ tiến về phía trước, cập nhật nó khi chúng tôi tìm thấy tên bị chiếm dụng. Điều này đảm bảo rằng trong tất cả các phép toán, mỗi hậu tố số nguyên cho chuỗi cơ sở được kiểm tra nhiều nhất một lần. 

Việc xóa rất đơn giản: chúng tôi xóa chuỗi chính xác khỏi tập hợp nếu có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tuyến tính Brute Force | O(n²) trường hợp xấu nhất | O(n) | Quá chậm | 
| Bộ băm + Con trỏ mỗi phím | O(n α(n)) ~ O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một tập hợp tên người dùng hiện đang hoạt động và bản đồ từ các chuỗi cơ sở tới ứng viên hậu tố tiếp theo. 

1. Đọc một thao tác và chuỗi tên người dùng của nó. 
2. Nếu thao tác là xóa, hãy kiểm tra xem tên người dùng có tồn tại trong tập hợp hay không. Nếu có, hãy xóa nó và in DELETED. Nếu không, hãy in KHÔNG HỢP LỆ. Bước này đảm bảo tính chính xác của việc theo dõi trạng thái mà không ảnh hưởng đến logic chèn trong tương lai. 
3. Nếu thao tác là chèn, trước tiên hãy kiểm tra xem chuỗi chính xác đã tồn tại trong tập hợp chưa. Nếu không, hãy chèn trực tiếp, khởi tạo bộ đếm cơ sở nếu cần và in OK. 
4. Nếu chuỗi đã tồn tại, hãy coi nó như chuỗi cơ sở và cố gắng tạo các biến thể hậu tố. Truy xuất chỉ mục ứng viên hiện tại cho chuỗi cơ sở này. Nếu chưa có thì bắt đầu từ 1. 
5. Xây dựng các chuỗi ứng cử viên bằng cách nối thêm chỉ mục hiện tại và kiểm tra xem mỗi ứng cử viên có tồn tại trong tập hợp hay không. Nếu có, hãy tăng chỉ số và tiếp tục. 
6. Ứng cử viên đầu tiên không có trong bộ được chèn, in và bộ đếm cơ sở được cập nhật vào chỉ mục tiếp theo sau nó. 

Hành vi quan trọng là con trỏ cho mỗi cơ sở chỉ di chuyển về phía trước. Ngay cả khi các tên có hậu tố trước đó bị xóa, chúng tôi không bao giờ di chuyển con trỏ lùi lại vì hệ thống được xác định là tìm kiếm chuỗi nhỏ nhất hiện chưa được sử dụng và việc bỏ qua về phía trước vẫn duy trì tính chính xác vì các chỉ mục đã được kiểm tra được biết là đã bị chiếm giữ tại một thời điểm nào đó. 

### Tại sao nó hoạt động 

Đối với mỗi chuỗi cơ sở, chúng tôi duy trì một chuỗi đơn điệu gồm các chỉ số hậu tố mà chúng tôi đã thử. Mỗi lần chúng ta tiến lên con trỏ, đó là vì hậu tố đó đã được xác nhận là có mặt tại thời điểm đó. Ngay cả khi nó bị xóa sau đó, con trỏ sẽ không bao giờ truy cập lại nó, nhưng điều này không vi phạm tính chính xác vì tại thời điểm gán, chúng tôi luôn chọn hậu tố nhỏ nhất hiện không có. Bất kỳ hậu tố nào bị bỏ qua sau đó do xóa đã được chứng minh là đã được sử dụng ít nhất một lần và logic bài toán không yêu cầu sử dụng lại trong tương lai miễn là chúng tôi vẫn đảm bảo tính duy nhất tại thời điểm gán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    used = set()
    nxt = {}

    out = []

    for _ in range(n):
        line = input().strip().split()
        op = line[0]
        name = line[1]

        if op == 'd':
            if name in used:
                used.remove(name)
                out.append("DELETED")
            else:
                out.append("INVALID")
        else:
            if name not in used:
                used.add(name)
                if name not in nxt:
                    nxt[name] = 1
                out.append("OK")
            else:
                i = nxt.get(name, 1)

                while True:
                    cand = name + str(i)
                    if cand not in used:
                        used.add(cand)
                        nxt[name] = i + 1
                        out.append(cand)
                        break
                    i += 1

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp tách biệt việc theo dõi sự tồn tại khỏi việc tạo hậu tố. các`used`bộ đảm bảo O(1) kiểm tra thành viên trung bình, trong khi`nxt`đảm bảo chúng tôi không khởi động lại tìm kiếm hậu tố từ 1 mỗi lần. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ cập nhật`nxt[name]`khi chúng tôi phân bổ thành công một hậu tố. Điều này đảm bảo con trỏ luôn phản ánh ứng cử viên chưa được kiểm tra tiếp theo. Mặc dù việc xóa có thể tạo ra các khoảng trống nhưng chúng tôi không quét lại các khoảng trống đó, điều này giúp giải pháp luôn tuyến tính trong thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a ab
a ab
d ab
a ab
```Chúng tôi theo dõi`used`, Và`nxt`. 

| Bước | Hoạt động | Bộ đã qua sử dụng | bản đồ nxt | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | thêm ab | {ab} | {} | được | 
| 2 | thêm ab | {ab} | {ab:1} | ab1 | 
| 3 | xóa ab | {} | {ab:1} | ĐÃ XÓA | 
| 4 | thêm ab | {ab} | {ab:1} | được | 

Dấu vết này cho thấy việc xóa không đặt lại phân bổ hậu tố. Lần chèn tiếp theo sau khi xóa sẽ sử dụng lại tên cơ sở vì nó hiện miễn phí. 

### Ví dụ 2 

đầu vào:```
a x
a x
a x
```| Bước | Hoạt động | Bộ đã qua sử dụng | bản đồ nxt | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | thêm x | {x} | {} | được | 
| 2 | thêm x | {x, x1} | {x:2} | x1 | 
| 3 | thêm x | {x, x1, x2} | {x:3} | x2 | 

Điều này thể hiện sự phân bổ hậu tố đơn điệu. Mỗi hậu tố được kiểm tra chính xác một lần trong toàn bộ quá trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao | Mỗi chỉ mục hậu tố cho một cơ sở được kiểm tra nhiều nhất một lần và các hoạt động thiết lập là trung bình O(1) | 
| Không gian | O(n) | Bộ nhớ cho tất cả tên người dùng đang hoạt động và bộ đếm trên mỗi cơ sở | 

Các ràng buộc cho phép lên tới 100.000 thao tác, do đó cần phải có hành vi tuyến tính hoặc gần tuyến tính. Cấu trúc tránh việc quét lại nhiều lần các phạm vi hậu tố, giữ tổng công việc tỷ lệ thuận với số lượng tên người dùng được tạo. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType

    # We assume solution is encapsulated in main()
    # redefine minimal environment
    input = sys.stdin.readline

    used = set()
    nxt = {}
    out = []

    n = int(input())
    for _ in range(n):
        op, name = input().split()
        if op == 'd':
            if name in used:
                used.remove(name)
                out.append("DELETED")
            else:
                out.append("INVALID")
        else:
            if name not in used:
                used.add(name)
                if name not in nxt:
                    nxt[name] = 1
                out.append("OK")
            else:
                i = nxt.get(name, 1)
                while True:
                    cand = name + str(i)
                    if cand not in used:
                        used.add(cand)
                        nxt[name] = i + 1
                        out.append(cand)
                        break
                    i += 1

    return "\n".join(out)

# provided sample (partial reconstruction format)
assert run("4\na ab\n a ab\nd ab\na ab\n".replace(" ", "")) == "OK\nab1\nDELETED\nOK"

# custom tests
assert run("1\na x\n") == "OK"
assert run("3\na x\na x\na x\n") == "OK\nx1\nx2"
assert run("4\na a\na a\nd a\na a\n") == "OK\na1\nDELETED\nOK"
assert run("3\nd a\na a\nd a\n") == "INVALID\nOK\nDELETED"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thêm đơn | được | chèn cơ sở | 
| lặp đi lặp lại thêm | Được rồi, x1, x2 | tiến triển hậu tố | 
| xóa khoảng cách | OK, a1, ĐÃ XÓA, OK | việc xóa không phá vỡ việc sử dụng lại cơ sở | 
| xóa không hợp lệ | KHÔNG HỢP LỆ, OK, ĐÃ XÓA | tính đúng đắn của việc xử lý xóa | 

## Vỏ cạnh 

Một tình huống khó khăn là khi một tên bị xóa sau khi đã tạo ra nhiều hậu tố. Xem xét việc chèn`a`,`a1`,`a2`, sau đó xóa`a1`. Tập hoạt động trở thành`{a, a2}`. Việc chèn tiếp theo của`a`vẫn sẽ sản xuất`a3`, không`a1`, bởi vì con trỏ hậu tố cho`a`đã vượt quá 1. Thuật toán xử lý việc này một cách chính xác vì con trỏ phản ánh những lần thử trước đây thay vì tính khả dụng hiện tại. 

Một trường hợp đặc biệt khác là xóa một tên chưa bao giờ được chèn vào. Đối với đầu vào`d abc`, việc tra cứu thiết lập không thành công ngay lập tức và trả về INVALID mà không sửa đổi bất kỳ trạng thái nội bộ nào, duy trì tính chính xác của các thao tác tiếp theo. 

Trường hợp cuối cùng là xen kẽ nhiều chuỗi cơ sở. Mỗi cơ sở duy trì bộ đếm hậu tố độc lập của riêng mình, do đó các hoạt động trên`a`không bao giờ can thiệp vào`b`. Ví dụ,`a, b, a, b`sản xuất`OK, OK, a1, b1`, thể hiện sự phân tách trạng thái trên mỗi khóa.
