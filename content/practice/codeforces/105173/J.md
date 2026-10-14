---
title: "CF 105173J - Bữa sáng"
description: "Chúng tôi được phục vụ một suất ăn duy nhất gồm hai loại món: bánh bao hấp và trứng. Mỗi chiếc bánh có giá cố định là 0,6 đơn vị tiền tệ và mỗi quả trứng có giá 1 đơn vị. Kích thước đơn hàng được xác định đầy đủ bằng hai số nguyên: số bánh và số trứng."
date: "2026-06-27T08:21:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105173
codeforces_index: "J"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Northeast), The 18th Northeast Collegiate Programming Contest"
rating: 0
weight: 105173
solve_time_s: 47
verified: true
draft: false
---

[CF 105173J - Bữa sáng](https://codeforces.com/problemset/problem/105173/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phục vụ một suất ăn duy nhất gồm hai loại món: bánh bao hấp và trứng. Mỗi chiếc bánh có giá cố định là 0,6 đơn vị tiền tệ và mỗi quả trứng có giá 1 đơn vị. Kích thước đơn hàng được xác định đầy đủ bằng hai số nguyên: số bánh và số trứng. Nhiệm vụ là tính tổng chi phí của bữa ăn cố định này và xuất ra dưới dạng số thập phân được làm tròn đến đúng hai chữ số sau dấu thập phân. 

Mặc dù định dạng đầu vào cho phép số nguyên nhưng phần quan trọng của vấn đề là độ chính xác số học. Kết quả đầu ra không phải là số nguyên nên việc xử lý bất cẩn định dạng dấu phẩy động có thể dễ dẫn đến kết quả trả lời sai chẳng hạn như khi in`39.2`thay vì`39.20`. 

Các ràng buộc loại bỏ mọi mối lo ngại về độ phức tạp của thuật toán. Chỉ có một trường hợp thử nghiệm và cả hai giá trị đều được cố định thành`n = 32`Và`m = 20`. Điều này có nghĩa là bất kỳ giải pháp nào thực hiện số học theo thời gian không đổi là đủ và ngay cả logic phân tích cú pháp dư thừa cũng sẽ không ảnh hưởng đến hiệu suất. 

Không có trường hợp cạnh cấu trúc nào về phạm vi hoặc đầu vào không hợp lệ. Rủi ro có ý nghĩa duy nhất là định dạng đầu ra dấu phẩy động không chính xác, đặc biệt là không bảo toàn hai chữ số thập phân hoặc tạo ra các tạo phẩm dấu phẩy động nhị phân. 

Một sai lầm ngây thơ sẽ là tính toán`32 * 0.6`sử dụng số học dấu phẩy động nhị phân và in kết quả thô. Ví dụ: trong Python,`0.6 * 32`có thể tạo ra một giá trị như`19.2`, bản in vẫn đẹp, nhưng ở các ngôn ngữ khác nó có thể tạo ra`19.199999...`. Một lỗi phổ biến khác là in mà không thực thi độ chính xác cố định, dẫn đến`39.2`thay vì`39.20`, được coi là không chính xác khi kiểm tra đầu ra nghiêm ngặt. 

## Phương pháp tiếp cận 

Cách giải thích mạnh mẽ của vấn đề là mô phỏng trực tiếp việc mua hàng: lặp lại từng chiếc bánh, thêm 0,6 vào tổng số đang chạy, sau đó lặp lại từng quả trứng và thêm 1,0 mỗi lần. Điều này đúng vì nó phản ánh chính xác định nghĩa về giá. Tuy nhiên, nó thực hiện 52 thao tác cộng, vốn không cần thiết do tính chất cố định của đầu vào. Quan trọng hơn, cách tiếp cận này không phù hợp về mặt cấu trúc vì nó bỏ qua rằng cả hai đại lượng đều đã biết trước và có thể nhân trực tiếp. 

Cách tiếp cận hiệu quả hơn nhận ra rằng tổng chi phí là biểu thức tuyến tính trong số lượng đầu vào. Thay vì cộng lại nhiều lần, chúng tôi tính toán chi phí dưới dạng tổng có trọng số đơn giản: bánh bao đóng góp`n * 0.6`và trứng đóng góp`m * 1.0`. Vì cả hai giá trị đều là hằng số trong trường hợp bài toán này nên toàn bộ quá trình tính toán sẽ giảm xuống còn một phép tính số học duy nhất, sau đó là định dạng. 

Quan sát quan trọng là cấu trúc không năng động. Không có việc ra quyết định, không có logic điều kiện và không có sự phụ thuộc giữa các mục. Mỗi đơn vị đóng góp độc lập và giống hệt nhau, cho phép tổng hợp thông qua phép nhân thay vì lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n + m) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tính toán công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên`n`Và`m`từ đầu vào. Chúng đại diện cho số lượng bánh và trứng cố định. 
2. Tính phần đóng góp của bánh bằng cách nhân`n`bằng 0,6. Điều này trực tiếp tổng hợp các chi phí giống nhau thay vì cộng từng chi phí một. 
3. Tính phần đóng góp của trứng bằng cách nhân`m`bằng 1,0. Bước này về mặt khái niệm là dư thừa về mặt hiệu ứng số học nhưng vẫn đảm bảo tính rõ ràng của mô hình chi phí. 
4. Cộng cả hai khoản đóng góp để có được tổng chi phí. 
5. Xuất kết quả được định dạng chính xác đến hai chữ số thập phân, đảm bảo giữ nguyên các số 0 ở cuối. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ tính tuyến tính của việc tích lũy chi phí. Mỗi mặt hàng đóng góp độc lập và bổ sung vào giá cuối cùng, do đó việc nhóm các mặt hàng giống hệt nhau không làm thay đổi kết quả. Việc thay thế phép cộng lặp lại bằng phép nhân sẽ giữ nguyên cấu trúc số học chính xác và định dạng ở cuối đảm bảo kết quả đầu ra khớp với biểu diễn thập phân được yêu cầu mà không làm thay đổi giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    total = n * 0.6 + m * 1.0
    print(f"{total:.2f}")

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai số nguyên, tính tổng có trọng số trực tiếp và in kết quả được định dạng. Chi tiết triển khai chính là việc sử dụng chuỗi ký tự được định dạng của Python với`:.2f`, đảm bảo chính xác hai chữ số sau dấu thập phân, bao gồm cả số 0 ở cuối. 

Không cần vòng lặp hoặc điều kiện vì cấu trúc bài toán hoàn toàn tĩnh. 

## Ví dụ đã hoạt động 

Kể từ khi tuyên bố chính thức sửa chữa`n = 32`Và`m = 20`, chúng tôi chứng minh cả trường hợp cố định được cung cấp và trường hợp giả định bổ sung để minh họa tính đúng đắn. 

Đối với đầu vào thực tế: 

| n | m | giá bánh | giá trứng | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 32 | 20 | 19.2 | 20.0 | 39,2 | 

Đầu ra được định dạng là`39.20`. Điều này cho thấy rằng mặc dù tính toán thô mang lại giá trị thập phân đơn, việc định dạng vẫn đảm bảo độ chính xác cần thiết. 

Đối với ví dụ thứ hai, hãy xem xét`n = 5`,`m = 3`: 

| n | m | giá bánh | giá trứng | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 5 | 3 | 3.0 | 3.0 | 6.0 | 

Đầu ra trở thành`6.00`, chứng tỏ rằng các số 0 ở cuối vẫn phải được giữ nguyên ngay cả khi phần thập phân bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc là tối thiểu, do đó giải pháp dễ dàng phù hợp với mọi giới hạn về thời gian và bộ nhớ hợp lý. Ngay cả trong các phiên bản tổng quát hơn của vấn đề, cấu trúc tương tự sẽ mở rộng mà không cần sửa đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, m = map(int, sys.stdin.readline().split())
    total = n * 0.6 + m * 1.0
    return f"{total:.2f}"

# provided case
assert run("32 20") == "39.20"

# minimum-like case
assert run("0 0") == "0.00"

# only buns
assert run("10 0") == "6.00"

# only eggs
assert run("0 10") == "10.00"

# mixed case
assert run("5 3") == "6.00"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 32 20 | 39,20 | tính chính xác và định dạng của trường hợp cố định chính thức | 
| 0 0 | 0,00 | xử lý bằng không và ổn định định dạng | 
| 10 0 | 6 giờ 00 | chỉ có một thành phần đóng góp | 
| 0 10 | 10:00 | tính toán đơn loại đối xứng | 
| 5 3 | 6 giờ 00 | độ đúng của tổ hợp tuyến tính tổng quát | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là định dạng đầu ra, đặc biệt đảm bảo rằng các giá trị như`39.2`được in dưới dạng`39.20`. Thuật toán tính toán một số dấu phẩy động chính xác về mặt toán học trong thang bài toán này, nhưng việc in ấn phải đảm bảo độ chính xác cố định. 

Đối với đầu vào`32 20`, quá trình tính toán tiến hành như`32 * 0.6 = 19.2`Và`20 * 1 = 20`, cho`39.2`. Nếu không có định dạng, một số ngôn ngữ hoặc câu lệnh in đơn giản sẽ xuất ra`39.2`, không kiểm tra được sự bình đẳng nghiêm ngặt. Với đầu ra được định dạng, kết quả sẽ trở thành`39.20`, thỏa mãn đặc tính kỹ thuật yêu cầu. 

Không có trường hợp biên nào khác tồn tại vì đầu vào cố định và không có điều kiện phân nhánh hoặc ràng buộc nào có thể khác nhau giữa các trường hợp thử nghiệm.
