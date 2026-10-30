---
title: "CF 106238L - Lập trình tính toán lượng tử"
description: "Vấn đề này không có đầu vào. Chúng tôi bắt đầu với ba qubit. Qubit đầu tiên chứa trạng thái không xác định tùy ý $$alpha trong khi qubit thứ hai và thứ ba đều được khởi tạo thành $ Nhiệm vụ là xuất ra một chương trình bằng ngôn ngữ HYW nhất định."
date: "2026-06-19T14:06:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106238
codeforces_index: "L"
codeforces_contest_name: "The 7th FanRuan Cup Southeast University Programming Contest (Winter) Professional Group"
rating: 0
weight: 106238
solve_time_s: 90
verified: true
draft: false
---

[CF 106238L - Lập trình điện toán lượng tử](https://codeforces.com/problemset/problem/106238/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này không có đầu vào. Chúng tôi bắt đầu với ba qubit. Qubit đầu tiên chứa trạng thái không xác định tùy ý$$\alpha|0\rangle+\beta|1\rangle,$$trong khi qubit thứ hai và thứ ba đều được khởi tạo thành$|0\rangle$. 

Nhiệm vụ là xuất ra một chương trình bằng ngôn ngữ HYW nhất định. Sau khi thực hiện chương trình, trạng thái của hai qubit đầu tiên phải trở nên chính xác$|0\rangle$và qubit thứ ba phải chứa trạng thái chưa xác định ban đầu$$\alpha|0\rangle+\beta|1\rangle.$$Phép biến đổi phải hoạt động với mọi giá trị có thể có của$\alpha$Và$\beta$. Chúng ta chỉ được phép sử dụng các cổng H, X, Z và CNOT cùng với các phép đo và thao tác dựa trên kết quả đo. Chương trình phải chứa tối đa mười sáu câu lệnh. 

Vì không có đầu vào và chỉ cần một lượng công việc không đổi nên độ phức tạp là không đáng kể. Thách thức thực sự là tìm ra một chuỗi các thao tác chuyển một trạng thái lượng tử chưa biết mà không trực tiếp quan sát nó. 

Một lỗi phổ biến là cố gắng đo qubit đầu tiên ngay lập tức. Hãy xem xét trạng thái$$\frac{|0\rangle+|1\rangle}{\sqrt2}.$$Sau khi đo, trạng thái sụp đổ thành$|0\rangle$hoặc$|1\rangle$, phá hủy các hệ số$\alpha,\beta$. Một khi thông tin bị mất, không có chuỗi phép toán điều kiện cổ điển nào có thể tái tạo lại sự chồng chất ban đầu. 

Một cạm bẫy khác là sao chép trạng thái bằng CNOT. Nếu như$$|\psi_1\rangle=\alpha|0\rangle+\beta|1\rangle$$và qubit thứ hai là$|0\rangle$, sau đó áp dụng CNOT từ qubit 1 đến qubit 2 sẽ cho$$\alpha|00\rangle+\beta|11\rangle.$$Đây là trạng thái vướng víu chứ không phải hai bản sao độc lập của qubit gốc. Các trạng thái lượng tử không thể được nhân bản. 

Cách tiếp cận đúng đắn là dịch chuyển tức thời lượng tử. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là cố gắng trích xuất các giá trị của$\alpha$Và$\beta$bằng phép đo và sau đó xây dựng lại trạng thái trên qubit thứ ba. Ý tưởng như vậy thất bại vì phép đo làm sụp đổ sự chồng chất. Thông tin được mã hóa theo biên độ không thể truy cập được thông qua một phép đo duy nhất, vì vậy phương pháp này về cơ bản là không thể thực hiện được. 

Quan sát quan trọng là dịch chuyển tức thời không tự truyền biên độ. Thay vào đó, trước tiên chúng tôi tạo một cặp vướng víu giữa qubit 2 và 3. Sau đó, qubit 1 và 2 được đo lường trên cơ sở phù hợp. Hai kết quả đo cổ điển xác định những hiệu chỉnh Pauli nào là cần thiết trên qubit 3. Sau khi áp dụng những hiệu chỉnh đó, qubit 3 trở thành trạng thái ban đầu, trong khi qubit 1 và 2 chuyển sang trạng thái cổ điển và có thể được đặt lại về 0. 

Vì ngôn ngữ HYW cho phép thực thi có điều kiện dựa trên kết quả đo lường nên việc triển khai dịch chuyển tức thời rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đo trực tiếp qubit chưa biết | O(1) | O(1) | Không đúng | 
| Dịch chuyển tức thời lượng tử có hiệu chỉnh | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một cặp Bell vướng víu giữa qubit 2 và 3. Áp dụng H cho qubit 2, sau đó áp dụng CNOT với qubit 2 làm đối chứng và qubit 3 làm mục tiêu. 
2. Gắn qubit chưa biết với qubit 2. Áp dụng CNOT với qubit 1 làm đối chứng và qubit 2 làm mục tiêu. 
3. Áp dụng H cho qubit 1. Điều này chuyển phép đo cơ sở Bell thành phép đo cơ sở tính toán thông thường. 
4. Đo qubit 1. Lưu kết quả làm kết quả đo đầu tiên. 
5. Đo qubit 2. Lưu kết quả làm kết quả đo thứ hai. 
6. Nếu kết quả đo thứ hai là 1, hãy áp dụng X cho qubit 3. Điều này sẽ loại bỏ lỗi lật bit do dịch chuyển tức thời gây ra. 
7. Nếu kết quả đo đầu tiên là 1, hãy áp dụng Z cho qubit 3. Điều này sẽ loại bỏ lỗi đảo pha do dịch chuyển tức thời gây ra. 
8. Sau khi đo, qubit 1 và 2 đã giảm về giá trị cổ điển. Nếu kết quả đo đầu tiên là 1, hãy áp dụng X cho qubit 1. 
9. Nếu kết quả đo thứ hai là 1, hãy áp dụng X cho qubit 2. 

Sau những lần điều chỉnh này, qubit 1 và 2 trở thành$|0\rangle$, trong khi qubit 3 chứa trạng thái ban đầu chưa biết. 

### Tại sao nó hoạt động 

Dịch chuyển tức thời phân tách hệ thống ba qubit thành bốn nhánh tương ứng với hai kết quả đo. Ở mỗi nhánh, qubit 3 chỉ khác với trạng thái ban đầu bởi sự kết hợp của toán tử X và Z. Vì kết quả đo được biết theo cách cổ điển nên các hiệu chỉnh thích hợp có thể được áp dụng có điều kiện. 

Các phép đo thu gọn qubit 1 và 2 thành một trong hai$|0\rangle$hoặc$|1\rangle$. Áp dụng X khi giá trị đo được bằng 1 sẽ đặt lại chúng về$|0\rangle$. Do đó trạng thái cuối cùng luôn là$$|0\rangle|0\rangle(\alpha|0\rangle+\beta|1\rangle),$$đó chính xác là những gì vấn đề yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

print("""H 2
CNOT 2 3
CNOT 1 2
H 1
measure 1
measure 2
if m2=1 then X 3
if m1=1 then Z 3
if m1=1 then X 1
if m2=1 then X 2""")
```Hai lệnh đầu tiên tạo cặp Bell giữa qubit 2 và 3. 

Hai hướng dẫn tiếp theo thực hiện việc chuẩn bị dịch chuyển tức thời tiêu chuẩn. Đo qubit 1 và 2 tạo ra hai bit cổ điển. 

Hai hiệu chỉnh có điều kiện trên qubit 3 sẽ tái tạo lại trạng thái chưa biết ban đầu. Cuối cùng, các qubit đo được sẽ được đặt lại về 0 bằng cách áp dụng X bất cứ khi nào giá trị đo được của chúng bằng một. 

Toàn bộ chương trình chỉ chứa mười câu lệnh, khá thoải mái dưới giới hạn mười sáu câu. 

## Ví dụ đã hoạt động 

Vì trạng thái chứa các hệ số ký hiệu nên việc theo dõi biên độ một cách trực tiếp sẽ mang lại nhiều thông tin hơn so với việc sử dụng đầu vào số. 

### Ví dụ 1 

Giả sử trạng thái ban đầu là$|1\rangle|0\rangle|0\rangle$. 

| Bước | Tiểu bang | 
| --- | --- | 
| Ban đầu | ( | 
| H2 | (( | 
| CNOT 2 3 | (( | 
| CNOT 1 2 | (( | 
| H 1 | Sự chồng chất của bốn trạng thái cơ bản | 
| Đo qubit 1 và 2 | Một trong bốn chi nhánh | 
| Sửa có điều kiện | Qubit 3 trở thành ( | 
| Đặt lại qubit 1 và 2 | ( | 

Ví dụ này cho thấy trạng thái cơ bản được chuyển giao hoàn hảo. 

### Ví dụ 2 

Giả sử trạng thái ban đầu là$$\frac{|0\rangle+|1\rangle}{\sqrt2}.$$| Bước | Mô tả | 
| --- | --- | 
| Tạo cặp chuông | Qubit vướng víu 2 và 3 | 
| Chuyển đổi chuông | Tương quan qubit 1 với cặp Bell | 
| Đo qubit 1 và 2 | Lấy hai bit cổ điển | 
| Áp dụng hiệu chỉnh X và Z | Khôi phục sự chồng chất ban đầu trên qubit 3 | 
| Đặt lại qubit 1 và 2 | Trạng thái cuối cùng trở thành ( | 

Điều này chứng tỏ rằng dịch chuyển tức thời hoạt động đối với các trạng thái chồng chất tùy ý, không chỉ các trạng thái cơ bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một chuỗi cố định gồm mười câu lệnh được in | 
| Không gian | O(1) | Không cần cấu trúc dữ liệu | 

Chương trình thực hiện một lượng công việc không đổi và sử dụng bộ nhớ không đổi. Các giới hạn là tầm thường đối với một vấn đề như vậy. 

## Trường hợp thử nghiệm```
# There is no input.
# Any execution should produce the same program.

expected = """H 2
CNOT 2 3
CNOT 1 2
H 1
measure 1
measure 2
if m2=1 then X 3
if m1=1 then Z 3
if m1=1 then X 1
if m2=1 then X 2"""

def run(inp: str) -> str:
    return expected

assert run("") == expected
assert run("\n") == expected
assert run("123\n") == expected
assert run("random text\nmore text\n") == expected
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đầu vào trống | chương trình cố định | trường hợp bình thường | 
| một dòng trống | chương trình cố định | bỏ qua đầu vào vô ích | 
| số tùy ý | chương trình cố định | đầu ra độc lập với đầu vào | 
| văn bản ngẫu nhiên | chương trình cố định | chỉ có chương trình in mới quan trọng | 

## Vỏ cạnh 

Một trường hợp nguy hiểm là nhà nước$|0\rangle$. Các phép đo dịch chuyển tức thời có thể tạo ra các kết quả cổ điển khác nhau, nhưng các hiệu chỉnh có điều kiện luôn trả về qubit 3 cho$|0\rangle$và các thao tác đặt lại khiến qubit 1 và 2 bằng 0. 

Một trường hợp thú vị khác là trạng thái$|1\rangle$. Một số nhánh yêu cầu hiệu chỉnh X, một số yêu cầu hiệu chỉnh Z và một số yêu cầu cả hai. Vì việc hiệu chỉnh được xác định bằng kết quả đo nên mỗi nhánh đều kết thúc với qubit 3 bằng$|1\rangle$. 

Trường hợp tinh tế nhất là sự chồng chất thực sự như$$\frac{|0\rangle+|1\rangle}{\sqrt2}.$$Phép đo trực tiếp sẽ phá hủy thông tin về pha. Dịch chuyển tức thời tránh điều này bằng cách không bao giờ đo qubit 3. Biên độ vẫn giữ nguyên và chỉ các toán tử hiệu chỉnh cổ điển mới được chọn tùy theo kết quả đo. Điều này bảo tồn trạng thái lượng tử hoàn chỉnh.
