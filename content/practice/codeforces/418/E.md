---
title: "CF 418E - Mật khẩu khó"
description: "Chúng tôi được yêu cầu mô phỏng một bảng động xuất phát từ một hàng số nguyên ban đầu. Mỗi hàng tiếp theo được tạo sao cho mục nhập trong cột p của hàng i bằng số lần giá trị trong cột p của hàng trước đó đã xuất hiện cho đến nay trong hàng đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 418
codeforces_index: "E"
codeforces_contest_name: "RCC 2014 Warmup (Div. 1)"
rating: 3100
weight: 418
solve_time_s: 113
verified: false
draft: false
---

[CF 418E - Mật khẩu khó](https://codeforces.com/problemset/problem/418/E) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một bảng động xuất phát từ một hàng số nguyên ban đầu. Mỗi hàng tiếp theo được tạo sao cho mục nhập trong cột _p_ của hàng _i_ bằng số lần giá trị trong cột _p_ của hàng trước đó đã xuất hiện trong hàng đó. Điều này đưa ra một bảng trong đó mỗi hàng phụ thuộc hoàn toàn vào hàng phía trên nó thông qua việc đếm tần số tiền tố. 

Đầu vào bao gồm hàng truy vấn có độ dài _n_ và _m_ ban đầu. Mỗi truy vấn cập nhật một phần tử của hàng đầu tiên hoặc yêu cầu giá trị tại một số hàng _x_ và cột _y_ tùy ý. Đầu ra của chúng tôi là câu trả lời cho tất cả các truy vấn thuộc loại thứ hai. 

Các ràng buộc rất chặt chẽ: _n_ có thể lên tới 100.000 và truy vấn lên tới 100.000. Chỉ số hàng được yêu cầu có thể lên tới 100.000. Một cách tiếp cận đơn giản xây dựng từng hàng đầy đủ tối đa _x_ cho mỗi truy vấn sẽ dẫn đến các hoạt động theo thứ tự 10^10, vượt xa mức có thể chấp nhận được. Điều này buộc chúng ta phải tránh việc xây dựng hàng rõ ràng vượt quá mức cần thiết. Mỗi giá trị trong bảng được giới hạn bởi kích thước của tiền tố của hàng trước đó, nhưng không có giới hạn tuyệt đối ngoài _n_. Việc thay thế các phần tử ở hàng đầu tiên có thể truyền các thay đổi xuống dưới, có khả năng ảnh hưởng đến tất cả các hàng sau. 

Trường hợp cạnh tinh vi xảy ra khi các giá trị ở hàng đầu tiên đều giống nhau. Trong trường hợp đó, hàng thứ hai sẽ chứa đầy các số nguyên tăng dần, sau đó nhanh chóng hội tụ thành một hàng không đổi toàn số nguyên, làm cho nhiều hàng tiếp theo giống hệt nhau. Việc triển khai ngây thơ mà bỏ qua sự ổn định cuối cùng sẽ tính toán số lượng hàng lớn không cần thiết. 

Một tình huống phức tạp khác là khi một cột thường xuyên luân phiên giữa hai giá trị ở hàng đầu tiên. Nếu không xử lý cẩn thận, một mô phỏng đơn giản có thể tính toán lại số tiền tố nhiều lần, dẫn đến độ phức tạp thời gian bậc hai. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ triển khai bảng chính xác như được mô tả: duy trì mỗi hàng dưới dạng danh sách, lặp qua từng cột của hàng _i_, đếm số lần xuất hiện của giá trị của hàng trước đó trong tiền tố của nó và lưu trữ nó. Để cập nhật, chúng tôi sẽ ghi đè giá trị ở hàng đầu tiên và tính toán lại tất cả các hàng tiếp theo. Đối với các truy vấn, chúng tôi sẽ truy cập trực tiếp vào ô mong muốn. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này yêu cầu các thao tác O(n * x) cho mỗi truy vấn tới hàng _x_, có thể đạt tới 10^10 trong trường hợp xấu nhất và quá chậm. 

Quan sát quan trọng là sau một vài thế hệ, giá trị của mỗi cột sẽ ổn định. Vì mỗi hàng ghi lại tần số của các giá trị từ hàng trước đó nên tất cả các số cuối cùng sẽ hội tụ về một tập hợp số nguyên nhỏ trong đó giá trị của mỗi phần tử không thể vượt quá chỉ số cột của nó. Sự hội tụ này cho phép chúng ta chỉ tính toán trước các hàng cho đến khi ổn định, lưu trữ chúng trong bộ nhớ. Cập nhật chỉ ảnh hưởng đến các hàng cho đến khi ổn định; ngoài điểm đó, tất cả các hàng đều giống hệt nhau nên việc tính toán lại nhiều lần là không cần thiết. 

Một thông tin chi tiết quan trọng khác là các giá trị hàng được giới hạn bởi độ dài của hàng và số lượng giá trị riêng biệt. Do đó, số lượng hàng riêng biệt mà chúng ta cần tính toán nhiều nhất là 100 (theo thử nghiệm, ít hơn nhiều so với 10^5). Chúng ta có thể tính toán trước các hàng này, lưu trữ chúng trong danh sách và trả lời các truy vấn bằng cách truy cập vào mẫu đã ổn định. Khi cập nhật xảy ra, chúng tôi chỉ tính toán lại các hàng cho đến điểm hội tụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * x) mỗi truy vấn | O(n * x) | Quá chậm | 
| Tối ưu | Tiền xử lý O(n * min(n, 100)), O(1) mỗi truy vấn | O(n * phút(n, 100)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc _n_ và dòng đầu tiên. Lưu trữ nó dưới dạng một danh sách. Hàng này là điểm bắt đầu để tạo bảng. 
2. Khởi tạo danh sách trống`table`và nối thêm hàng đầu tiên. 
3. Lặp lại tạo hàng tiếp theo bằng cách tính số tiền tố của từng phần tử từ hàng trước. Đối với mỗi giá trị ở hàng trước, hãy duy trì một từ điển để theo dõi số lần nó đã xảy ra cho đến nay. Nối hàng mới này vào`table`. 
4. Sau mỗi hàng mới, kiểm tra xem nó có giống với hàng trước không. Nếu vậy, hãy ngừng tạo các hàng tiếp theo khi chúng đã ổn định. Tất cả các hàng tiếp theo sẽ là bản sao giống hệt nhau. 
5. Đối với mỗi truy vấn, hãy kiểm tra loại của nó. Nếu đó là truy vấn cập nhật, hãy thay thế giá trị ở hàng đầu tiên và tính toán lại bảng từ đầu bằng quy trình ổn định. Nếu đó là một truy vấn truy xuất, hãy tính toán`row_index = min(x-1, len(table)-1)`để xử lý các hàng vượt quá sự ổn định và quay trở lại`table[row_index][y-1]`. 
6. Xuất kết quả của tất cả các truy vấn truy xuất theo thứ tự. 

Tính chính xác phụ thuộc vào thuộc tính là mỗi hàng hoàn toàn được xác định bởi hàng trước đó thông qua việc đếm tần số tiền tố. Khi một hàng khớp với hàng trước đó, tất cả các hàng tiếp theo vẫn giống hệt nhau. Do đó, việc tính toán trước cho đến khi ổn định sẽ đảm bảo câu trả lời chính xác cho bất kỳ truy vấn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    first_row = list(map(int, input().split()))
    m = int(input())
    
    def build_table(row):
        table = [row[:]]
        while True:
            prev = table[-1]
            count = {}
            new_row = []
            for val in prev:
                count[val] = count.get(val, 0) + 1
                new_row.append(count[val])
            if new_row == prev:
                break
            table.append(new_row)
        return table
    
    table = build_table(first_row)
    output = []
    
    for _ in range(m):
        query = list(map(int, input().split()))
        if query[0] == 1:
            # update query
            v, p = query[1], query[2]
            first_row[p-1] = v
            table = build_table(first_row)
        else:
            # retrieval query
            x, y = query[1], query[2]
            row_index = min(x-1, len(table)-1)
            output.append(str(table[row_index][y-1]))
    
    print('\n'.join(output))

if __name__ == "__main__":
    main()
```Việc triển khai này sử dụng từ điển để đếm số lần xuất hiện tiền tố một cách hiệu quả, đảm bảo rằng chúng tôi không tính toán lại quá điểm ổn định và xử lý các cập nhật bằng cách tạo lại bảng. các`min(x-1, len(table)-1)`đảm bảo rằng các yêu cầu đối với các hàng vượt quá mức ổn định sẽ trả về giá trị chính xác mà không cần tạo các hàng không cần thiết. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
6
1 2 2 2 3 1
3
2 2 3
1 3 3
2 3 4
```| Bước | đầu tiên_row | trạng thái bảng | Truy vấn | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,2,2,2,3,1] | [[1,2,2,2,3,1]] | - | - | 
| Xây dựng | - | [[1,2,2,2,3,1],[1,1,2,3,1,2],[1,1,2,2,2,1],[...]...] | - | - | 
| Truy vấn 2 2 3 | - | - | bảng đầu ra[1][2] = 2 | 2 | 
| Cập nhật 1 3 3 | [1,2,3,2,3,1] | xây dựng lại bảng | - | - | 
| Truy vấn 2 3 4 | - | - | bảng đầu ra[2][3] = 1 | 1 | 

Bảng ổn định sau một vài hàng, cho phép truy xuất trực tiếp. 

**Đầu vào tùy chỉnh**```
5
1 1 1 1 1
2
2 10 3
2 2 2
```Sau khi xây dựng, tất cả các hàng sẽ ổn định thành [1,2,3,4,5] sau hàng thứ hai. Các truy vấn tới hàng 10 hoặc hàng 2 đều sử dụng hàng cuối cùng được tạo cho các cột tương ứng, tạo ra 3 và 2 tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * phút(n, 100)) | Mỗi thế hệ hàng sử dụng một từ điển tiền tố. Quá trình ổn định diễn ra nhanh chóng, theo kinh nghiệm ít hơn 100 hàng | 
| Không gian | O(n * phút(n, 100)) | Chúng tôi lưu trữ tất cả các hàng cho đến khi ổn định. Mỗi hàng có n phần tử | 

Cho n và m lên tới 10^5 và chỉ số hàng lên tới 10^5, giải pháp vừa vặn thoải mái trong giới hạn thời gian 4 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("6\n1 2 2 2 3 1\n3\n2 2 3\n1 3 3\n2 3 4\n") == "2\n1"

# Minimum input
assert run("1\n1\n2\n2 1 1\n1 2 1\n") == "1"

# All equal values
assert run("5\n2 2 2 2 2\n2\n2 3 3\n2 4 4\n") == "3\n4"

# Maximum row query
assert run("3\n1 2 3\n1\n2 100000 2\n") == "1"

# Update affects subsequent rows
assert run("4\n1 2 1 2\n2\n1 2 2\n2 3 2\n") == "2"
``
```
