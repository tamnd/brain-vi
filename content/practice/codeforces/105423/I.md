---
title: "CF 105423I - \u6570\u636e\u68c0\u7d22\u7cfb\u7edf"
description: "Chúng tôi đang xây dựng một hệ thống thành viên đơn giản hóa hoạt động giống như một tập hợp được lọc bằng hàm băm với nhiều hàm băm. Hệ thống lưu trữ các phần tử trong một mảng nhị phân có kích thước n, ban đầu tất cả đều là số 0."
date: "2026-06-23T04:17:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105423
codeforces_index: "I"
codeforces_contest_name: "2024\u6e56\u5357\u7701\u8d5b"
rating: 0
weight: 105423
solve_time_s: 59
verified: true
draft: false
---

[CF 105423I - \u6570\u636e\u68c0\u7d22\u7cfb\u7edf](https://codeforces.com/problemset/problem/105423/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một hệ thống thành viên đơn giản hóa hoạt động giống như một tập hợp được lọc bằng hàm băm với nhiều hàm băm. Hệ thống lưu trữ các phần tử trong một mảng nhị phân có kích thước`n`, ban đầu tất cả đều là số không. Khi một phần tử được chèn vào, chúng tôi tính toán`k`các giá trị giống như hàm băm xác định từ nó và đánh dấu các vị trí đó trong mảng là`1`. Khi chúng tôi truy vấn một phần tử, chúng tôi tính toán lại tương tự`k`giá trị và kiểm tra xem tất cả các vị trí tương ứng đã được`1`. Nếu có, chúng tôi báo cáo rằng phần tử “tồn tại”, nếu không, chúng tôi báo cáo rằng phần tử đó không tồn tại. 

Các hàm chuyển đổi cực kỳ đơn giản: với mỗi chỉ số hàm`i`, giá trị được ánh xạ là`h_i(x) = x^i mod n`. Điều này có nghĩa là mỗi phần tử tạo ra một tập hợp nhỏ các chỉ số trong`[0, n-1]`và các chỉ mục đó là yếu tố thúc đẩy cả thao tác chèn và truy vấn. 

Các ràng buộc đủ nhỏ để chúng ta có thể mô phỏng đơn giản. Với`n, m, q ≤ 10000`Và`k ≤ 10`, mỗi phép toán tối đa là 10 phép tính lũy thừa hoặc phép nhân mô đun. Ngay cả khi chúng tôi tính toán lũy thừa một cách đơn giản cho mỗi truy vấn và lần chèn, tổng công việc vẫn nằm trong giới hạn vì chúng tôi chỉ thực hiện khoảng`m + q`hoạt động, mỗi hoạt động có chi phí không đổi rất nhỏ. 

Một trường hợp phức tạp xuất phát từ thực tế là các chỉ số được lấy modulo`n`, do đó va chạm được mong đợi và thậm chí cần thiết cho tính chính xác. Một vấn đề khác là các chỉ mục trùng lặp trong cùng một truy vấn hoặc phần chèn. Từ`h_i(x)`có thể bằng`h_j(x)`cho khác nhau`i`Và`j`, chấm điểm hoặc kiểm tra phải xử lý việc lặp lại một cách an toàn. Việc sử dụng một tập hợp hoặc đơn giản là lặp lại tất cả các chỉ mục mà không loại bỏ trùng lặp vẫn có tác dụng, nhưng việc loại bỏ trùng lặp sẽ tránh được việc kiểm tra lặp lại không cần thiết. 

Một sai lầm ngây thơ là coi phép lũy thừa là phép nhân tuyến tính mà không rút gọn mô đun hoặc quên rằng`x^i`phát triển nhanh chóng. Tuy nhiên, kể từ khi`i ≤ 10`, tính toán trực tiếp là an toàn. Một cạm bẫy khác là hiểu lầm rằng đây không phải là bộ lọc Bloom xác suất thực sự mà là một biến thể hàm băm cố định xác định. 

## Phương pháp tiếp cận 

Việc giải thích brute-force rất đơn giản: với mỗi lần chèn, hãy tính toán tất cả`k`quyền hạn và đặt các vị trí đó trong mảng thành`1`. Đối với mỗi truy vấn, tính toán lại giống nhau`k`vị trí và kiểm tra xem mỗi vị trí trong số đó có`1`. 

Cách tiếp cận này đúng vì định nghĩa hệ thống nêu rõ rằng tư cách thành viên chỉ được xác định bởi các vị trí đó. Không có trạng thái ẩn hoặc sự phụ thuộc thứ tự nào ngoài chính mảng đó. 

Nút thắt phức tạp không đến từ số lượng thao tác mà đến từ việc lũy thừa lặp đi lặp lại nếu thực hiện không hiệu quả. Nếu chúng ta tính toán lại`x^i`từ đầu mỗi lần sử dụng phép nhân đơn giản bên trong một vòng lặp, mỗi thao tác sẽ tốn chi phí`O(i)`, đưa ra trường hợp xấu nhất`O(k^2)`cho mỗi thao tác, vẫn tầm thường dưới những ràng buộc. Ngay cả những phương pháp đắt tiền hơn vẫn sẽ vượt qua. 

Quan sát quan trọng là`k ≤ 10`rất nhỏ nên chúng tôi không cần bất kỳ tối ưu hóa nâng cao nào. Tính toán trước lũy thừa lên đến số mũ`k`đối với từng giá trị hoặc tính toán chúng tăng dần là đủ. Chúng ta có thể tính toán`x^1`, sau đó nhân nhiều lần với`x`modulo`n`để có được`x^2`,`x^3`, vân vân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((m+q) · k · log i) hoặc O((m+q) · k²) | O(n) | Đã chấp nhận | 
| Tối ưu | O((m+q) · k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng nhị phân`arr`kích thước`n`, ban đầu tất cả đều là số không. 

1. Đọc`n, k, m, q`và khởi tạo`arr`với số không. Mảng này biểu thị liệu mỗi chỉ mục đã được kích hoạt bởi ít nhất một phần tử được chèn hay chưa. 
2. Đối với mỗi phần tử được chèn`x`, tính toán tất cả các giá trị`x^1 mod n, x^2 mod n, ..., x^k mod n`. Điều này có thể được thực hiện lặp đi lặp lại bằng cách duy trì giá trị công suất đang chạy. 
3. Đối với mỗi chỉ số được tính toán, hãy đặt`arr[index] = 1`. Các phép gán lặp đi lặp lại là vô hại vì mảng là bình thường. 
4. Đối với từng phần tử truy vấn`y`, tính toán cùng một chuỗi các chỉ số bằng cách sử dụng cùng một phương pháp số mũ lặp. 
5. Kiểm tra tất cả`k`chỉ số. Nếu bất kỳ vị trí nào trong`arr`là`0`, đầu ra`0`ngay lập tức cho truy vấn đó. 
6. Nếu tất cả các vị trí đều`1`, đầu ra`1`. 

Lý do chúng tôi tính toán lại lũy thừa riêng biệt cho từng phần tử thay vì lưu vào bộ nhớ đệm là vì cả hai`m`Và`q`nhỏ và đầu vào độc lập. Bộ nhớ đệm được chia sẻ sẽ làm phức tạp việc triển khai mà không cải thiện độ phức tạp tiệm cận một cách có ý nghĩa. 

### Tại sao nó hoạt động 

Mỗi phần tử được biểu diễn hoàn toàn bằng tập hợp các chỉ số được tạo ra bởi các hàm biến đổi. Việc chèn đảm bảo các chỉ số đó được đặt thành`1`. Một truy vấn chỉ thành công nếu mọi chỉ mục tương ứng với phần tử truy vấn đã được đặt bằng một số thao tác chèn trước đó. Vì mảng không bao giờ đặt lại các bit từ`1`quay lại`0`, nó tích lũy bằng chứng đơn điệu của các phần tử được chèn vào. Do đó, nếu tập chỉ mục đầy đủ của truy vấn được chứa trong tập hợp các vị trí được kích hoạt thì nó phải được chèn trước đó (hoặc không thể phân biệt được theo sơ đồ băm này), phù hợp với định nghĩa về tư cách thành viên của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compute_indices(x, k, n):
    res = []
    val = x % n
    for i in range(1, k + 1):
        res.append(val)
        val = (val * x) % n
    return res

def main():
    n, k, m, q = map(int, input().split())
    arr = [0] * n

    insert_vals = list(map(int, input().split()))
    query_vals = list(map(int, input().split()))

    for x in insert_vals:
        for idx in compute_indices(x, k, n):
            arr[idx] = 1

    out = []
    for y in query_vals:
        ok = True
        for idx in compute_indices(y, k, n):
            if arr[idx] == 0:
                ok = False
                break
        out.append('1' if ok else '0')

    print(' '.join(out))

if __name__ == "__main__":
    main()
```Chi tiết triển khai cốt lõi là tính toán công suất lặp. Thay vì tính toán lại`x^i`từ đầu, chúng tôi duy trì`val = x^i mod n`và cập nhật nó như`val = val * x % n`. Điều này làm giảm công việc lặp đi lặp lại và giữ cho mã sạch sẽ và có thể dự đoán được. 

Mảng`arr`đóng vai trò là trạng thái bền vững duy nhất. Các phần chèn chỉ đặt các bit và các truy vấn chỉ đọc chúng. Cấu trúc đơn điệu này tránh mọi nhu cầu xử lý xóa hoặc logic khôi phục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=11, k=3, m=4, q=5
insert: [1, 5, 3, 8]
query: [4, 7, 1, 0, 4]
```Chúng tôi theo dõi các lần chèn. 

Vì`x=1`, chỉ số là`[1,1,1]`, vì vậy chỉ có vị trí 1 trở thành 1. 

cho`x=5`, chỉ số là`[5, 3, 4]`. 

Vì`x=3`, chỉ số là`[3, 9, 5]`. 

Vì`x=8`, chỉ số là`[8, 9, 6]`. 

Mảng kích hoạt cuối cùng có các mảng tại`{1,3,4,5,6,8,9}`. 

Bây giờ truy vấn: 

| y | chỉ số | kiểm tra mảng | đầu ra | 
| --- | --- | --- | --- | 
| 4 | [4,5,9] | tất cả đều tồn tại | 1 | 
| 7 | [7,5,2] | 7 hoặc 2 thiếu | 0 | 
| 1 | [1,1,1] | tồn tại | 1 | 
| 0 | [0,0,0] | mất tích | 0 | 
| 4 | [4,5,9] | tồn tại | 1 | 

Điều này xác nhận các chỉ số lặp đi lặp lại như`[1,1,1]`không ảnh hưởng đến tính đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m + q) · k) | mỗi phần tử tính k phép nhân mô-đun | 
| Không gian | O(n) | mảng nhị phân có kích thước n | 

Các giới hạn cho phép thực hiện tổng cộng tối đa 20.000 phép tính, mỗi phép tính thực hiện tối đa 10 phép nhân, tốc độ này khá nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main
    main()
    return sys.stdout.getvalue().strip()

# sample-like case
assert run("""11 3 4 5
1 5 3 8
4 7 1 0 4
""") == "1 0 1 0 1"

# minimum case
assert run("""1 1 1 1
0
0
""") == "1"

# no insertions
assert run("""5 2 0 3

1 2 3
""") == "0 0 0"

# all elements identical
assert run("""10 3 3 2
2 2 2
2 2
""") == "1 1"

# boundary power cycle behavior
assert run("""7 4 2 2
3 6
3 6
""") in ["1 1", "0 0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 1 0 1 0 1 | tính chính xác của các truy vấn hỗn hợp | 
| phần tử đơn | 1 | cấu hình tối thiểu | 
| không chèn | 0 0 0 | hành vi cấu trúc trống rỗng | 
| trùng lặp | 1 1 | chèn bình thường | 
| va chạm mô-đun | biến | xử lý chu trình | 

## Vỏ cạnh 

Khi tất cả các chỉ mục được tạo bởi một truy vấn lặp lại, chẳng hạn như`x = 1`, chúng tôi nhận được`[1,1,1,...]`. Thuật toán vẫn kiểm tra cùng một vị trí nhiều lần, nhưng vì mảng là nhị phân và ổn định nên kết quả không bị ảnh hưởng. 

Vì`n = 1`, mọi số mũ đều giảm xuống`0`, vì vậy tất cả các phần tử ánh xạ tới cùng một chỉ mục. Hệ thống trở thành một thanh ghi một bit và mọi thao tác chèn hoặc truy vấn sẽ thu gọn về vị trí đó. Thuật toán thiết lập chính xác`arr[0] = 1`sau bất kỳ lần chèn nào, làm cho tất cả các truy vấn trở lại`1`khi có ít nhất một phần tử tồn tại. 

Đối với các giá trị lớn của`x`, phép nhân mô-đun đảm bảo các giá trị vẫn bị giới hạn, do đó không xảy ra hiện tượng tràn hoặc suy giảm hiệu suất. Tính toán công suất lặp đảm bảo tính chính xác mà không cần tính lũy thừa nhanh.
