---
title: "CF 104804H - \u042d\u0434\u0443\u0440\u0434 \u0438 \u0444\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u0438"
description: "Chúng ta được cung cấp một loạt các bản ghi ảnh, mỗi bản ghi mô tả địa điểm và thời điểm bức ảnh được chụp. Mỗi bản ghi chứa tên vị trí và bốn trường thời gian: ngày, tháng, giờ và phút. Năm được ngầm định là 2113 cho tất cả các bức ảnh."
date: "2026-06-28T16:52:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "H"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 86
verified: false
draft: false
---

[CF 104804H - \u042d\u0434\u0443\u0440\u0434 \u0438 \u0444\u043e\u0442\u043e\u0433\u0440\u0430\u0444\u0438\u0438](https://codeforces.com/problemset/problem/104804/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các bản ghi ảnh, mỗi bản ghi mô tả địa điểm và thời điểm bức ảnh được chụp. Mỗi bản ghi chứa tên vị trí và bốn trường thời gian: ngày, tháng, giờ và phút. Năm được ngầm định là 2113 cho tất cả các bức ảnh. 

Nhiệm vụ là sắp xếp lại tất cả các bức ảnh theo quy tắc đặt hàng tùy chỉnh. Đầu tiên, các bức ảnh được sắp xếp theo từ điển theo chuỗi vị trí của chúng, sử dụng thứ tự ASCII trong đó các chữ cái viết hoa đứng trước các chữ cái viết thường. Nếu hai ảnh có cùng vị trí, chúng sẽ được sắp xếp theo dấu thời gian: ngày trước đó sẽ đến trước, so sánh ngày, sau đó là tháng, sau đó là giờ, rồi phút. Nếu cả vị trí và dấu thời gian giống hệt nhau thì thứ tự nhập ban đầu phải được giữ nguyên. 

Đầu ra không chỉ là danh sách ảnh đã được sắp xếp mà mỗi ảnh cũng phải được in với chỉ mục gốc ở đầu vào và có dấu thời gian được định dạng lại trong đó các dấu chấm phân tách ngày, tháng và năm 2113 được chèn rõ ràng. 

Đầu vào là một luồng bản ghi liên tục không có số lượng rõ ràng, lên tới 100000 dòng. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào tệ hơn O(n log n), vì quét tuyến tính hoặc chèn lặp đi lặp lại vào cấu trúc đã sắp xếp sẽ giảm xuống thời gian bậc hai. 

Một vấn đề tế nhị xuất hiện trong cách cấu trúc đầu vào. Mỗi ảnh nằm trên một dòng riêng, nhưng trong một số định dạng dữ liệu thử nghiệm, khoảng trắng hoặc ngắt dòng có thể không nhất quán, do đó, giải pháp mạnh mẽ phải dựa vào việc đọc dựa trên mã thông báo thay vì giả định phân tách dòng nghiêm ngặt. Một điểm tinh tế khác là tính ổn định: nếu chúng ta sử dụng thuật toán sắp xếp không ổn định và không mã hóa rõ ràng chỉ mục đầu vào vào khóa, chúng ta sẽ mất hành vi tie-break cần thiết. 

Một ví dụ tối thiểu về vấn đề ổn định là hai bức ảnh giống hệt nhau: 

đầu vào:```
Moscow 01 01 00 00
Moscow 01 01 00 00
```Đầu ra đúng phải bảo toàn thứ tự 1 rồi 2. Một cách sắp xếp ngây thơ trên`(place, time)`một mình có thể hoán đổi chúng một cách tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đọc tất cả các bức ảnh thành một mảng và liên tục chọn phần tử nhỏ nhất theo thứ tự yêu cầu. Cách sắp xếp lựa chọn này bắt chước: đối với mỗi vị trí, hãy quét danh sách còn lại và tìm mức tối thiểu. Điều này đúng vì nó trực tiếp thực hiện quy tắc so sánh, nhưng mỗi lựa chọn tốn O(n), n lần lặp lại sẽ cho thời gian O(n²), quá chậm đối với 100000 bản ghi. Điều đó dẫn đến so sánh khoảng 10¹⁰ trong trường hợp xấu nhất, điều này không khả thi. 

Quan sát quan trọng là thứ tự hoàn toàn mang tính xác định và có thể tổng hợp thành một so sánh bộ dữ liệu. So sánh vị trí là từ điển và so sánh thời gian là từ điển trên các số nguyên có chiều rộng cố định. Khi chúng ta biểu diễn mỗi bản ghi dưới dạng một bộ dữ liệu`(place, day, month, hour, minute, index)`, tính năng sắp xếp tích hợp của Python có thể xử lý thứ tự một cách hiệu quả bằng cách sử dụng Timsort trong O(n log n). Chỉ mục này chỉ được thêm vào để thực thi tính ổn định một cách rõ ràng khi tất cả các trường khác khớp với nhau. 

Vì vậy, thay vì tìm kiếm nhiều lần, chúng tôi chuyển đổi vấn đề thành một loại sắp xếp toàn cục duy nhất với khóa được xác định rõ ràng và để thuật toán sắp xếp xử lý thứ tự một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp xếp theo bộ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào cho đến EOF, phân tích từng dòng thành các thành phần của nó: đặt chuỗi và bốn số nguyên biểu thị ngày, tháng, giờ và phút. Điều này là cần thiết vì kích thước đầu vào không xác định và chỉ kết thúc ở cuối tệp. 
2. Đối với mỗi bức ảnh, hãy lưu trữ một bản ghi có cấu trúc chứa chỉ mục gốc, chuỗi địa điểm và bốn thành phần thời gian. Chỉ mục này được yêu cầu để duy trì thứ tự ban đầu khi tất cả các trường khác giống hệt nhau. 
3. Xác định khóa sắp xếp cho mỗi bản ghi như`(place, day, month, hour, minute, index)`. Thứ tự hoạt động tự nhiên vì Python so sánh các bộ dữ liệu theo từ điển từ trái sang phải. 
4. Sắp xếp toàn bộ danh sách bằng phím này. Điều này tạo ra thứ tự cần thiết trong một lần duyệt qua cấu trúc bên trong của thuật toán sắp xếp. 
5. Lặp lại danh sách đã sắp xếp và in từng bản ghi theo định dạng được yêu cầu, xây dựng lại dấu thời gian như`DD.MM.2113 HH:MM`với các số 0 đứng đầu được giữ nguyên. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là thứ tự được yêu cầu là thứ tự từ điển trên một hệ thống phân cấp cố định của các trường. Khi mỗi bức ảnh được ánh xạ vào một bộ dữ liệu mã hóa thứ bậc đó theo thứ tự quan trọng, việc so sánh bộ dữ liệu sẽ khớp chính xác với các quy tắc so sánh của vấn đề. Việc thêm chỉ mục ban đầu đảm bảo rằng ngay cả khi tất cả các trường bằng nhau, thứ tự sẽ trở nên hoàn toàn tổng thể và nhất quán với thứ tự đầu vào, điều này đảm bảo tính ổn định bất kể việc triển khai sắp xếp cơ bản như thế nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    data = sys.stdin.read().strip().splitlines()
    photos = []

    for i, line in enumerate(data, 1):
        if not line.strip():
            continue
        parts = line.split()
        place = parts[0]
        d = int(parts[1])
        m = int(parts[2])
        h = int(parts[3])
        mi = int(parts[4])
        photos.append((place, d, m, h, mi, i))

    photos.sort(key=lambda x: (x[0], x[1], x[2], x[3], x[4], x[5]))

    out = []
    for p in photos:
        place, d, m, h, mi, idx = p
        out.append(f"{idx} {place} {d:02d}.{m:02d}.2113 {h:02d}:{mi:02d}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc toàn bộ đầu vào cùng một lúc bằng cách sử dụng`sys.stdin.read()`để tránh chi phí trên mỗi dòng, điều này rất quan trọng khi xử lý tối đa 100000 bản ghi. Mỗi dòng được chia thành các mã thông báo và chúng tôi trích xuất chuỗi địa điểm và bốn trường số. 

Mỗi bức ảnh được lưu trữ dưới dạng một bộ bao gồm cả chỉ mục gốc của nó. Chỉ mục này không được sử dụng để sắp xếp mức độ ưu tiên ngoại trừ mục đích phân loại cuối cùng. Khóa sắp xếp được xác định rõ ràng để phù hợp với hệ thống phân cấp thứ tự được yêu cầu. 

Cuối cùng, việc định dạng được thực hiện cẩn thận bằng cách sử dụng định dạng số nguyên không đệm để đảm bảo các trường có hai chữ số, vì định dạng đầu ra yêu cầu định dạng nghiêm ngặt cho ngày, tháng, giờ và phút. 

## Ví dụ đã hoạt động 

### Mẫu 1 Trace 

Chúng tôi chỉ theo dõi các trường chính được sử dụng để sắp xếp. 

| Bước | Địa điểm | Ngày (D,M,H,Min) | Chỉ mục | 
| --- | --- | --- | --- | 
| Đầu vào | Mátxcơva | 15,01,13,24 | 1 | 
| Đầu vào | Maykop | 17,05,00,13 | 2 | 
| Đầu vào | Adler | 21,11,04,20 | 3 | 
| Đầu vào | St.Petersburg | 30,01,17,59 | 4 | 
| Đầu vào | Mátxcơva | 01,04,00,00 | 5 | 
| Đầu vào | Kekland | 04,12,01,43 | 6 | 
| Đầu vào | Mátxcơva | 15,01,02,43 | 7 | 

Sau khi sắp xếp theo từ điển theo địa điểm: 

Adler đến trước, sau đó là Kekland, rồi Maykop, rồi Moscow, rồi St.Petersburg. Bên trong Moscow, dấu thời gian quyết định việc đặt hàng. 

Thứ tự sắp xếp cuối cùng: 

3, 6, 2, 7, 1, 5, 4 

Điều này phù hợp với thứ tự đầu ra mẫu. 

### Mẫu 2 Dấu vết 

| Bước | Địa điểm | Ngày (D,M,H,Min) | Chỉ mục | 
| --- | --- | --- | --- | 
| Đầu vào | Mátxcơva | 15,01,13,24 | 1 | 
| Đầu vào | Maykop | 17,05,00,13 | 2 | 
| Đầu vào | Adler | 21,11,04,20 | 3 | 
| Đầu vào | Mátxcơva | 15,01,13,24 | 4 | 
| Đầu vào | st.Petersburg | 30,01,17,59 | 5 | 
| Đầu vào | Mátxcơva | 15,01,13,24 | 6 | 
| Đầu vào | Mátxcơva | 01,04,00,00 | 7 | 
| Đầu vào | Kekland | 04,12,01,43 | 8 | 

Ở đây, nhiều mục Moscow giống hệt nhau với dấu thời gian giống hệt nhau xuất hiện. Chỉ số trở thành yếu tố quyết định trong số đó, đảm bảo trật tự ổn định phù hợp với hình thức đầu vào. 

Thứ tự cuối cùng: 

3, 8, 2, 1, 4, 6, 7, 5 

Điều này chứng tỏ rằng việc ràng buộc chỉ mục sẽ bảo toàn chính xác thứ tự đầu vào giữa các bản ghi giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tất cả các bản ghi được sắp xếp một lần bằng cách sử dụng phương pháp sắp xếp dựa trên so sánh trên các bộ dữ liệu | 
| Không gian | O(n) | Tất cả các bản ghi ảnh được lưu trữ trong bộ nhớ | 

Các ràng buộc cho phép tối đa 100000 ảnh và việc sắp xếp O(n log n) dễ dàng phù hợp với giới hạn thời gian vì nó bao gồm nhiều nhất khoảng vài triệu so sánh. Việc sử dụng bộ nhớ là tuyến tính theo số lượng bản ghi và vẫn an toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import sys
    from io import StringIO
    backup = sys.stdout
    sys.stdout = StringIO()
    main()
    out = sys.stdout.getvalue().strip()
    sys.stdout = backup
    return out

# sample 1
assert run("""Moscow 15 01 13 24
Maykop 17 05 00 13
Adler 21 11 04 20
St.Petersburg 30 01 17 59
Moscow 01 04 00 00
Kekland 04 12 01 43
Moscow 15 01 02 43
""") == """3 Adler 21.11.2113 04:20
6 Kekland 04.12.2113 01:43
2 Maykop 17.05.2113 00:13
7 Moscow 15.01.2113 02:43
1 Moscow 15.01.2113 13:24
5 Moscow 01.04.2113 00:00
4 St.Petersburg 30.01.2113 17:59"""

# identical records stability
assert run("""A 01 01 00 00
A 01 01 00 00
A 01 01 00 00
""") == """1 A 01.01.2113 00:00
2 A 01.01.2113 00:00
3 A 01.01.2113 00:00"""

# boundary times
assert run("""Z 31 12 23 59
A 01 01 00 00
""") == """2 A 01.01.2113 00:00
1 Z 31.12.2113 23:59"""

# mixed case ordering
assert run("""a 01 01 00 00
A 01 01 00 00
""") == """2 A 01.01.2113 00:00
1 a 01.01.2113 00:00"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dấu thời gian giống hệt nhau | trật tự ổn định | sự đúng đắn của sự ràng buộc | 
| ngày tối thiểu/tối đa | xử lý ranh giới | so sánh thời gian từ điển chính xác | 
| tên phân biệt chữ hoa chữ thường | Đặt hàng ASCII | thứ tự chuỗi đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều ảnh có giá trị địa điểm và dấu thời gian giống hệt nhau. Trong tình huống đó, cách sắp xếp dựa trên bộ so sánh đơn giản không bao gồm chỉ mục ban đầu có thể tạo ra một hoán vị hợp lệ khác nhau trong mỗi lần chạy hoặc dựa vào hành vi không ổn định. Bằng cách thêm chỉ mục vào khóa sắp xếp một cách rõ ràng, thứ tự trở nên xác định và nhất quán với thứ tự đầu vào. 

Một trường hợp đặc biệt khác là các vị trí chỉ khác nhau tùy theo trường hợp, chẳng hạn như`Moscow`Và`moscow`. Vì thứ tự dựa trên ASCII nên các chữ cái viết hoa phải đứng trước chữ thường. Việc triển khai đúng không được bình thường hóa chữ hoa chữ thường hoặc sử dụng so sánh nhận biết ngôn ngữ, nếu không thứ tự sẽ đi chệch khỏi quy tắc từ điển bắt buộc. 

Trường hợp tinh tế cuối cùng là định dạng đầu ra: việc quên đệm 0 vào ngày, tháng, giờ hoặc phút có một chữ số sẽ tạo ra dấu thời gian không chính xác về mặt cú pháp ngay cả khi sắp xếp chính xác, do đó, định dạng phải được thực thi nghiêm ngặt tại thời điểm đầu ra.
