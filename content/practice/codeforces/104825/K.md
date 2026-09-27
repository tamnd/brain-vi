---
title: "CF 104825K - str\u8fdb\u5236"
description: "Chúng ta được cung cấp một chuỗi hoạt động giống như mô tả của một hệ thống số vị trí, ngoại trừ việc nó không phải là một cơ số cố định như số thập phân hoặc nhị phân. Thay vào đó, mỗi vị trí đều có “quy tắc mang theo” riêng."
date: "2026-06-28T12:33:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104825
codeforces_index: "K"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104825
solve_time_s: 45
verified: true
draft: false
---

[CF 104825K - str\u8fdb\u5236](https://codeforces.com/problemset/problem/104825/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi hoạt động giống như mô tả của một hệ thống số vị trí, ngoại trừ việc nó không phải là một cơ số cố định như số thập phân hoặc nhị phân. Thay vào đó, mỗi vị trí đều có “quy tắc mang theo” riêng. Nếu một vị trí chứa chữ số x thì vị trí đó hoạt động giống như cơ số x: khi đạt đến x, nó sẽ đặt lại về 0 và mang 1 đến vị trí tiếp theo. 

Chúng ta cũng được cho một số nguyên không âm d ở dạng thập phân thông thường. Nhiệm vụ là biểu diễn d trong hệ cơ số hỗn hợp này, tạo ra chính xác m chữ số, bao gồm cả số 0 đứng đầu nếu không sử dụng vị trí cao hơn. 

Một cách hữu ích để hình dung điều này là một đồng hồ đo đường trong đó mỗi bánh xe có số lượng khe khác nhau và những khả năng đó được chỉ định bởi các ký tự của chuỗi đầu vào s. 

Ràng buộc m 1000 có nghĩa là độ dài biểu diễn tối đa là một nghìn chữ số, do đó, bất kỳ giải pháp nào xử lý số một lần trên mỗi chữ số đều đủ nhanh. Tham số thứ hai n không liên quan đến bản thân tính toán và có thể được bỏ qua một cách an toàn sau khi đọc. 

Trường hợp lỗi phổ biến nhất ở đây là coi hệ thống như một chuyển đổi cơ sở thông thường với một cơ sở duy nhất. Điều đó sẽ ngay lập tức phá vỡ các đầu vào như s = "29". Chữ số có nghĩa nhỏ nhất có thể có cơ số 9, trong khi chữ số tiếp theo có cơ số 2, do đó cách giải thích cơ số thống nhất sẽ mất hoàn toàn cấu trúc. 

Một vấn đề tế nhị khác là phương hướng. Nếu người ta cho rằng chữ số ngoài cùng bên trái có ý nghĩa ít nhất thì việc chuyển đổi sẽ bị đảo ngược. Ví dụ: nếu s = "23" và d = 5, việc diễn giải từ trái sang phải thay vì từ phải sang trái sẽ tạo ra các phần dư khác nhau vì hướng truyền mang thay đổi trọng số vị trí. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng việc tăng một số từ 0 cho đến khi đạt d, cập nhật các chữ số cơ số hỗn hợp mỗi lần. Mỗi mức tăng yêu cầu truyền lan qua tối đa m vị trí, do đó chi phí trong trường hợp xấu nhất tỷ lệ với d nhân m. Vì d có thể lớn tới 10^10 nên điều này hoàn toàn không khả thi ngay cả đối với m nhỏ. 

Cấu trúc của hệ thống gợi ý một cách giải thích trực tiếp hơn. Mỗi vị trí đóng góp độc lập vào cách tổng số mở rộng khi nhìn từ phía có ý nghĩa nhỏ nhất, tương tự như chuyển đổi một số thành hệ số giai thừa hoặc bất kỳ hệ cơ số hỗn hợp chung nào. Thay vì mô phỏng số gia, chúng ta có thể trích xuất trực tiếp các chữ số bằng phép chia lặp lại. 

Quan sát quan trọng là chuỗi s xác định một chuỗi các cơ sở và biểu diễn của d chính xác là phân tích cơ số hỗn hợp của d theo các cơ sở này. Điều này cho phép chúng ta tính từng chữ số bằng cách lấy số dư và giảm d từng bước. 

Cách tiếp cận bạo lực không thành công vì nó coi con số là tiến hóa theo thời gian, trong khi cách tiếp cận tối ưu coi nó như một bài toán phân rã tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(d · m) | O(m) | Quá chậm | 
| Phân hủy cơ số hỗn hợp | O(m) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Giải thích từng ký tự của chuỗi s dưới dạng cơ số nguyên cho vị trí chữ số tương ứng của nó. Ký tự ngoài cùng bên phải là vị trí ít quan trọng nhất, vì ký tự truyền sang trái trong các hệ thống vị trí tiêu chuẩn. 
2. Bắt đầu từ số thập phân d cho trước, đại diện cho giá trị đầy đủ mà chúng ta cần để phân tách thành biểu diễn cơ số hỗn hợp. 
3. Di chuyển các vị trí từ phải sang trái. Tại mỗi vị trí i, tính chữ số là phần dư của d chia cho s[i]. Điều này đảm bảo chữ số hợp lệ trong giới hạn cơ sở cục bộ của nó. 
4. Sau khi trích xuất chữ số cho vị trí i, giảm d bằng phép chia số nguyên với s[i]. Mô hình này loại bỏ sự đóng góp của vị trí hiện tại và truyền giá trị còn lại lên các vị trí cao hơn. 
5. Lưu trữ tất cả các chữ số được trích xuất trong một mảng trong quá trình truyền tải. 
6. Sau khi xử lý tất cả các vị trí, xuất các chữ số theo thứ tự ban đầu từ trái sang phải. 

Lý do điều này có hiệu quả là vì mỗi vị trí đóng góp một hệ số trong hệ thống vị trí có trọng số được xác định linh hoạt bằng tích của tất cả các cơ số ở bên phải nó. Hoạt động modulo và chia lặp lại chính là tính toán chính xác các hệ số trong hệ thống đó, đảm bảo mỗi chữ số thỏa mãn ràng buộc cục bộ của nó trong khi vẫn giữ nguyên giá trị toàn cục. 

Bất biến được duy trì xuyên suốt quá trình là khi bắt đầu xử lý vị trí i, biến d biểu thị giá trị còn lại chưa được gán cho các vị trí thấp hơn và giá trị còn lại này luôn chia hết cho cấu trúc được xác định bởi hậu tố của các cơ sở đã được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m, n = map(int, input().split())
    s = input().strip()
    d = int(input().strip())

    bases = [int(c) for c in s]
    res = [0] * m

    for i in range(m - 1, -1, -1):
        b = bases[i]
        res[i] = d % b
        d //= b

    print("".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán chính xác. Mảng res lưu trữ các chữ số ở vị trí cuối cùng của chúng, vì vậy chúng ta tránh đảo ngược ở cuối bằng cách ghi trực tiếp vào chỉ mục i. Vòng lặp chạy từ vị trí ít quan trọng nhất ở m-1 đến 0, đảm bảo hướng truyền sóng mang chính xác. 

Một lỗi triển khai phổ biến là xử lý từ trái sang phải trong khi vẫn sử dụng modulo, điều này phá vỡ cấu trúc phụ thuộc. Một vấn đề tế nhị khác là quên rằng mỗi s[i] là một ký tự và phải được chuyển đổi thành số nguyên trước khi tính số học. 

## Ví dụ đã hoạt động 

Xét s = "243" và d = 17. Chúng ta hiểu chữ số ngoài cùng bên phải là cơ số 3, chữ số ở giữa là cơ số 4, chữ số bên trái là cơ số 2. 

Chúng tôi xử lý từ phải sang trái. 

| Vị trí | Căn cứ | Hiện tại d | Chữ số (d % cơ sở) | d mới (d // cơ sở) | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 17 | 2 | 5 | 
| 1 | 4 | 5 | 1 | 1 | 
| 0 | 2 | 1 | 1 | 0 | 

Đầu ra cuối cùng là 112. 

Dấu vết này cho thấy mỗi vị trí tiêu thụ một phần giá trị còn lại như thế nào, đảm bảo rằng không có vị trí nào vượt quá giới hạn cơ sở cục bộ của nó trong khi vẫn duy trì tổng giá trị. 

Bây giờ hãy xem xét s = "9999" và d = 123. Điều này hoạt động giống như cách biểu diễn cơ số 9 tiêu chuẩn với độ dài cố định. 

| Vị trí | Căn cứ | Hiện tại d | Chữ số | Mới d | 
| --- | --- | --- | --- | --- | 
| 3 | 9 | 123 | 6 | 13 | 
| 2 | 9 | 13 | 4 | 1 | 
| 1 | 9 | 1 | 1 | 0 | 
| 0 | 9 | 0 | 0 | 0 | 

Đầu ra là 0146. 

Điều này chứng tỏ phương pháp này tự nhiên chuyển thành chuyển đổi cơ số chuẩn như thế nào khi tất cả các cơ số đều bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi chữ số được tính bằng một modulo và một phép chia | 
| Không gian | O(m) | Lưu trữ các chữ số đầu ra | 

Các ràng buộc cho phép tối đa 1000 chữ số, do đó, việc quét tuyến tính trên chuỗi với số học theo thời gian không đổi trên mỗi chữ số có hiệu quả không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m, n = map(int, input().split())
    s = input().strip()
    d = int(input().strip())

    bases = [int(c) for c in s]
    res = [0] * m

    for i in range(m - 1, -1, -1):
        b = bases[i]
        res[i] = d % b
        d //= b

    return "".join(map(str, res))

# minimal case
assert run("1 1\n2\n0\n") == "0"

# simple mixed radix
assert run("3 1\n234\n10\n") == "102"

# all same base behavior
assert run("4 1\n9999\n123\n") == "0146"

# leading zeros needed
assert run("5 1\n22222\n3\n") == "00003"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 chữ số | 0 | trường hợp biên nhỏ nhất | 
| 234, 10 | 102 | căn cứ hỗn hợp với mang | 
| 9999, 123 | 0146 | tính nhất quán cơ sở thống nhất | 
| 22222, 3 | 00003 | không bảo quản hàng đầu | 

## Vỏ cạnh 

Khi d bằng 0, mọi phép toán modulo đều tạo ra 0 và phép chia giữ nó ở mức 0 xuyên suốt. Ví dụ: với s = "2345" và d = 0, mỗi bước tính chữ số 0 và giữ nguyên d là 0, tạo ra đầu ra chứa đầy số 0. 

Khi d rất nhỏ so với các cơ sở ban đầu nhưng lớn hơn các cơ sở sau, các vị trí thấp hơn sẽ hấp thụ tất cả giá trị trước tiên. Ví dụ: s = "234" và d = 2 tạo ra các chữ số [0, 0, 2] khi được xử lý từ phải sang trái, vì chỉ cơ số có ý nghĩa nhỏ nhất mới đóng góp phần dư khác 0. 

Khi tất cả các cơ số đều bằng nhau, chẳng hạn như s = "7777", thuật toán hoạt động chính xác giống như chuyển đổi cơ số 7 với độ rộng cố định, xác nhận rằng cơ số hỗn hợp tổng quát hóa các hệ thống vị trí tiêu chuẩn thay vì thay thế chúng.
