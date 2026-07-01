---
title: "CF 104400C - Xây dựng hoán vị"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến n để tối đa hóa biểu thức tuyệt đối xen kẽ lồng nhau có dạng trong đó các khác biệt được lặp đi lặp lại giữa các phần tử liên tiếp với giá trị tuyệt đối được áp dụng ở mỗi cấp độ trừ."
date: "2026-06-30T23:00:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104400
codeforces_index: "C"
codeforces_contest_name: "Hunan University 2023 the 19th Programming Contest"
rating: 0
weight: 104400
solve_time_s: 47
verified: true
draft: false
---

[CF 104400C - Xây dựng hoán vị](https://codeforces.com/problemset/problem/104400/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến n để tối đa hóa biểu thức tuyệt đối xen kẽ lồng nhau có dạng trong đó các khác biệt được lặp đi lặp lại giữa các phần tử liên tiếp với giá trị tuyệt đối được áp dụng ở mỗi cấp độ trừ. Về mặt khái niệm, chúng ta sắp xếp các con số sao cho khi đánh giá “chuỗi trừ tuyệt đối xen kẽ” này, giá trị cuối cùng càng lớn càng tốt. 

Đầu vào là một số nguyên n và đầu ra là bất kỳ hoán vị nào có độ dài n làm cực đại hóa biểu thức này. Vì đầu ra là một hoán vị nên mọi số nguyên từ 1 đến n phải xuất hiện đúng một lần. 

Ràng buộc n 10^6 ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng đánh giá hàm mục tiêu theo các hoán vị. Số lượng hoán vị là n!, quá lớn ngay cả với n = 20. Ngay cả khi cố gắng mô phỏng biểu thức cho một hoán vị cố định là O(n), do đó, việc ép buộc tất cả các hoán vị một cách thô bạo sẽ là O(n · n!), điều này hoàn toàn không khả thi. 

Một trường hợp khó nhận thấy là cấu trúc biểu thức phụ thuộc rất nhiều vào thứ tự, không chỉ vào nhiều tập hợp. Ví dụ: với n = 3, các hoán vị như [1, 3, 2] và [3, 1, 2] tạo ra các giá trị tuyệt đối trung gian khác nhau mặc dù chúng chứa các phần tử giống nhau. Cách tiếp cận hoán đổi cục bộ tham lam có thể dễ dàng thất bại vì các quyết định sớm ảnh hưởng đến tất cả các hoạt động còn lại. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu cách hoạt động của phép trừ tuyệt đối xen kẽ. Biểu thức liên tục lấy giá trị hiện tại rồi trừ phần tử tiếp theo, sau đó gói kết quả vào một giá trị tuyệt đối và tiếp tục. 

Nếu chúng tôi mở rộng cấu trúc, chúng tôi nhận thấy rằng kết quả cuối cùng bị ảnh hưởng nặng nề bởi cách các giá trị lớn được đặt sớm trong chuỗi và cách sử dụng các giá trị nhỏ để “khuếch đại” sự khác biệt thông qua việc đảo dấu trước khi các giá trị tuyệt đối làm sụp đổ cấu trúc. 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị, tính toán biểu thức theo thời gian O(n) mỗi lần và theo dõi mức tối đa. Điều này đúng nhưng ngay lập tức bị hỏng vì nó yêu cầu hoán vị O(n!), điều này là không thể đối với n lên đến 10^6. 

Quan sát quan trọng là giá trị tuyệt đối sẽ hủy bỏ thông tin dấu hiệu ở mỗi bước, có nghĩa là về cơ bản chúng ta đang kiểm soát độ lớn của các kết quả trung gian bằng cách chọn thời điểm xảy ra bước nhảy lớn. Để tối đa hóa giá trị cuối cùng, chúng tôi muốn buộc biểu thức liên tục tạo ra những khác biệt lớn thay vì loại bỏ chúng. Điều này đạt được bằng cách xen kẽ giữa các giá trị lớn và nhỏ sao cho mỗi bước trừ tạo ra độ lớn trước khi áp dụng giá trị tuyệt đối. 

Cấu trúc này dẫn đến một cấu trúc tối ưu đơn giản: đặt số lớn nhất lên đầu tiên, sau đó xen kẽ các số còn lại ở các đầu đối diện của dãy. Điều này đảm bảo rằng mỗi bước trừ sẽ tạo ra một khoảng cách lớn giữa giá trị hiện tại và phần tử được chọn tiếp theo, tránh hiệu ứng hủy bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Xây dựng xen kẽ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chiến lược tối ưu là xây dựng một hoán vị bằng cách xen kẽ giữa giá trị còn lại lớn nhất và giá trị nhỏ nhất còn lại.

1. Khởi tạo hai con trỏ low = 1 và high = n. Chúng tôi khẳng định rằng tất cả các số trong [thấp, cao] đều chưa được sử dụng. 
2. Bắt đầu xây dựng danh sách câu trả lời. Chúng ta bắt đầu bằng cách lấy giá trị lớn nhất còn lại, cao và nối thêm giá trị đó. Sau đó chúng tôi giảm cao vì nó hiện được sử dụng. Bắt đầu từ giá trị lớn nhất đảm bảo có những khoảng trống lớn sớm trong biểu thức. 
3. Tiếp theo, chúng ta lấy giá trị nhỏ nhất còn lại ở mức thấp và nối thêm giá trị đó, sau đó tăng ở mức thấp. Điều này tạo ra sự tương phản mạnh mẽ với giá trị lớn trước đó. 
4. Ta tiếp tục luân phiên: lấy từ cao, xuống thấp, lặp đi lặp lại cho đến khi sử dụng hết số. 
5. Nếu một con trỏ vượt qua con trỏ kia, hãy dừng lại. Điều này xử lý cả n chẵn và lẻ một cách tự nhiên. 

Lý do chúng tôi thay thế là vì những khác biệt cực đoan liên tiếp sẽ tối đa hóa độ lớn của các kết quả trung gian trước khi áp dụng giá trị tuyệt đối. Nếu chúng ta sắp xếp các số theo thứ tự được sắp xếp, thì sự khác biệt sẽ nhỏ và số hủy sẽ chiếm ưu thế, làm giảm giá trị cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước của hoán vị được xây dựng, giá trị được chọn tiếp theo là giá trị cực trị của khoảng còn lại. Điều này đảm bảo rằng sự khác biệt giữa các phần tử được chọn liên tiếp luôn lớn nhất có thể với những gì còn lại. Vì biểu thức áp dụng giá trị tuyệt đối sau mỗi phép trừ nên dấu của các kết quả trung gian không quan trọng mà chỉ quan trọng độ lớn của chênh lệch ở mỗi bước. Việc xây dựng đảm bảo rằng mỗi bước tạo ra một khoảng cách gần như tối đa có thể và không có sự sắp xếp lại các phần tử còn lại có thể làm tăng bất kỳ chuyển đổi cục bộ nào mà không làm giảm chuyển tiếp khác. Điều này mang lại một chuỗi tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    l, r = 1, n
    res = []
    
    while l <= r:
        res.append(r)
        r -= 1
        if l <= r:
            res.append(l)
            l += 1
    
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai con trỏ biểu thị các giá trị nhỏ nhất và lớn nhất không được sử dụng. Mỗi lần lặp lại sẽ thêm mức tối đa hiện tại, sau đó là mức tối thiểu hiện tại, thu hẹp phạm vi có sẵn. Điều này trực tiếp mã hóa chiến lược xen kẽ được mô tả trước đó. Điều kiện vòng lặp đảm bảo chúng ta không bao giờ sử dụng lại các phần tử và kết quả in cuối cùng sẽ tạo ra một hoán vị hợp lệ. 

Một điểm tinh tế là chúng tôi luôn thêm giá trị cao trước. Bắt đầu bằng mức thấp sẽ tạo ra một hoán vị hợp lệ nhưng phá vỡ mẫu xen kẽ dự định nhằm tối đa hóa sự khác biệt ban đầu trong cấu trúc biểu thức. 

## Ví dụ đã hoạt động 

Xét n = 6. 

Chúng ta bắt đầu với l = 1, r = 6. 

| Bước | tôi | r | Được chọn | Hoán vị hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | 6 | [6] | 
| 2 | 2 | 6 | 1 | [6, 1] | 
| 3 | 2 | 5 | 5 | [6, 1, 5] | 
| 4 | 3 | 5 | 2 | [6, 1, 5, 2] | 
| 5 | 3 | 4 | 4 | [6, 1, 5, 2, 4] | 
| 6 | 4 | 4 | 3 | [6, 1, 5, 2, 4, 3] | 

Điều này cho thấy mức độ cực đoan được tiêu thụ dần dần như thế nào, đảm bảo mức độ lan truyền tối đa ở mỗi bước. 

Với n = 5, chúng ta tiến hành tương tự. 

| Bước | tôi | r | Được chọn | Hoán vị hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | [5] | 
| 2 | 2 | 5 | 1 | [5, 1] ​​| 
| 3 | 2 | 4 | 4 | [5, 1, 4] | 
| 4 | 3 | 4 | 2 | [5, 1, 4, 2] | 
| 5 | 3 | 3 | 3 | [5, 1, 4, 2, 3] | 

Trường hợp lẻ chứng tỏ phần tử ở giữa được đặt tự nhiên ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số từ 1 đến n được thêm đúng một lần | 
| Không gian | O(n) | Chúng tôi lưu trữ hoán vị kết quả | 

Thuật toán là tuyến tính và hoạt động thoải mái trong giới hạn ngay cả với n = 10^6, vì nó chỉ thực hiện một lần truyền với các cập nhật con trỏ theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    solve()
    
    return output.getvalue().strip()

# sample-style cases
assert run("1\n") == "1"
assert run("2\n") in ["2 1", "1 2"]

# small case
assert run("3\n") in ["3 1 2", "3 2 1"]

# even case
assert run("6\n") == "6 1 5 2 4 3"

# large structure sanity (just length check)
out = run("10\n").split()
assert sorted(map(int, out)) == list(range(1, 11))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | ranh giới tối thiểu | 
| n=2 | 2 1 hoặc 1 2 | trao đổi không tầm thường nhỏ nhất | 
| n=6 | 6 1 5 2 4 3 | sự đúng đắn của cấu trúc xen kẽ | 
| n=10 | hoán vị 1..10 | hiệu lực dưới những ràng buộc | 

## Vỏ cạnh 

Với n = 1, thuật toán đặt l = r = 1. Nó thêm 1 một lần và kết thúc ngay lập tức, tạo ra một hoán vị hợp lệ. 

Với n = 2, chúng ta bắt đầu với l = 1, r = 2. Thuật toán nối 2 rồi 1, tạo ra [2, 1]. Vòng lặp kết thúc vì con trỏ giao nhau. Điều này xác nhận quy tắc xen kẽ xử lý đầu vào có ý nghĩa nhỏ nhất mà không cần viết hoa đặc biệt. 

Đối với n lẻ như n = 5 thì phần tử cuối cùng còn lại là giá trị ở giữa. Khi l == r, nó được thêm một lần ở bước cuối cùng. Việc xây dựng không bao giờ bỏ qua phần tử này vì điều kiện vòng lặp cho phép sự bằng nhau.
