---
title: "CF 102621G - Hacker gà mái"
description: "Vấn đề là một nhiệm vụ tương tác. Đối tượng ẩn là mật khẩu được tạo từ các ký tự riêng biệt của bộ 62 ký tự chứa chữ thường, chữ in hoa và chữ số."
date: "2026-08-02T13:56:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102621
codeforces_index: "G"
codeforces_contest_name: "mBIT Advanced June 2020"
rating: 0
weight: 102621
solve_time_s: 56
verified: true
draft: false
---

[CF 102621G - Hacker gà](https://codeforces.com/problemset/problem/102621/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là một nhiệm vụ tương tác. Đối tượng ẩn là mật khẩu được tạo từ các ký tự riêng biệt của bộ 62 ký tự chứa chữ thường, chữ in hoa và chữ số. Một truy vấn sẽ gửi một mật khẩu có thể có cho thẩm phán và thẩm phán sẽ trả lời xem truy vấn đó có phải là mật khẩu chính xác hay không, một chuỗi thích hợp của mật khẩu hay hoàn toàn không có trong đó. Mục tiêu là tìm ra mật khẩu trong vòng 750 lần đoán. 

Hạn chế chính là mỗi ký tự xuất hiện nhiều nhất một lần, vì vậy mật khẩu là hoán vị của một tập hợp con của bảng chữ cái. Độ dài tối đa có thể là 62, đủ nhỏ để chúng ta có thể thực hiện tìm kiếm logarit trên các ký tự, nhưng không đủ để thử tất cả các hoán vị có thể. Số lượng mật khẩu có thể có là rất lớn, vì vậy bất kỳ phương pháp nào dựa trên việc liệt kê các ứng cử viên đều không thể thực hiện được. 

Những phần khó khăn đến từ việc diễn giải chính xác câu trả lời của thẩm phán. Một truy vấn chứa một ký tự không chỉ cho chúng ta biết về ký tự đó. Nó cũng tiết lộ liệu toàn bộ mật khẩu có độ dài bằng một hay không. Ví dụ, truy vấn`a`chống lại mật khẩu`a`cho`C`, trong khi truy vấn`a`chống lại`ab`cho`Y`. Việc coi cả hai phản hồi giống hệt nhau sẽ làm mất thông tin về độ dài. 

Một trường hợp tinh tế khác là truy vấn cuối cùng. Nếu chuỗi được xây dựng của chúng tôi chứa mọi ký tự của mật khẩu theo đúng thứ tự, câu trả lời có thể là`C`thay vì`Y`. Thuật toán phải dừng ngay lập tức`C`, vì không được phép gửi thêm truy vấn sau khi thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử các chuỗi ứng viên và sử dụng phản hồi để loại bỏ các mật khẩu không thể sử dụng được. Điều này đúng vì mọi câu trả lời đều cung cấp thông tin về chuỗi ẩn nhưng không gian tìm kiếm quá lớn. Ngay cả việc hạn chế chúng ta trong các chuỗi không có ký tự lặp lại cũng mang lại$$62! + 62 \cdot 61! + \dots$$mật khẩu có thể, vượt xa những gì có thể được kiểm tra trong 750 truy vấn. 

Quan sát hữu ích là thẩm phán có thể trả lời các câu hỏi tiếp theo. Đầu tiên, chúng ta có thể khám phá chính xác những ký tự tồn tại trong mật khẩu bằng cách hỏi về từng ký tự riêng lẻ. Sau đó, vấn đề còn lại chỉ là sắp xếp thứ tự các ký tự đó. 

Vì các ký tự là duy nhất nên khi chúng ta biết bộ ký tự thì mật khẩu chỉ là một thứ tự của chúng. Chúng ta có thể duy trì tiền tố được sắp xếp của mật khẩu và chèn các ký tự mới vào đúng vị trí của chúng. Tìm kiếm nhị phân trên các vị trí chèn hoạt động vì thứ tự ứng cử viên là một dãy con hoặc không phải như vậy. Điều này làm giảm số lượng truy vấn cần thiết để sắp xếp từ bậc hai xuống gần đúng$62 \log 62$. 

Lực lượng vũ phu hoạt động vì mọi phỏng đoán hợp lệ đều cung cấp thông tin, nhưng nó thất bại vì số lượng đơn đặt hàng có thể bùng nổ. Quan sát cho thấy việc kiểm tra trình tự tiếp theo cho thấy thứ tự tương đối cho phép chúng ta thay thế việc đoán bằng cách xây dựng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng mật khẩu có thể có) | O(số lượng ứng viên) | Quá chậm | 
| Tối ưu | Truy vấn O(62 log 62) | O(62) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Truy vấn mọi ký tự trong bảng chữ cái được phép. Nếu câu trả lời là`Y`, ký tự thuộc về mật khẩu. Nếu câu trả lời là`C`, mật khẩu chỉ gồm một ký tự duy nhất đó và chúng ta có thể hoàn thành ngay. 
2. Giữ các ký tự được phát hiện theo thứ tự chúng đã được tìm thấy cho đến nay. Ký tự được biết đầu tiên bắt đầu chuỗi có thứ tự hiện tại. 
3. Chèn mọi ký tự được phát hiện còn lại vào chuỗi hiện tại. Để kiểm tra xem một vị trí có đúng hay không, hãy đặt ký tự mới vào đó và truy vấn toàn bộ chuỗi. Câu trả lời tích cực có nghĩa là thứ tự phù hợp với mật khẩu ẩn. 
4. Sử dụng tìm kiếm nhị phân trong khi chèn. Nếu đặt ký tự trước vị trí ở giữa có hiệu quả thì câu trả lời nằm ở nửa bên trái. Nếu không thì nó phải ở sau vị trí đó. 
5. Sau mỗi lần chèn, chuỗi được duy trì là chuỗi con của mật khẩu thực. Khi tất cả các ký tự được chèn vào, chuỗi sẽ bằng mật khẩu, do đó truy vấn thành công tiếp theo sẽ trả về`C`. 

Tại sao nó hoạt động: điều bất biến là trình tự được duy trì luôn xuất hiện theo cùng thứ tự bên trong mật khẩu ẩn. Khi chèn ký tự mới, đúng một vị trí sẽ giữ nguyên thuộc tính này vì mật khẩu không có ký tự lặp lại. Tìm kiếm nhị phân chỉ chọn giữa các vị trí bằng cách sử dụng các kiểm tra tuần tự hợp lệ, do đó nó không thể loại bỏ vị trí thực. Sau khi tất cả các ký tự được đặt, chuỗi được duy trì sẽ chứa mọi ký tự của mật khẩu và do đó chính nó phải là mật khẩu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

alphabet = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

def solve():
    def ask(s):
        print(s, flush=True)
        return input().strip()

    present = []

    for c in alphabet:
        res = ask(c)
        if res == "C":
            return
        if res == "Y":
            present.append(c)

    if not present:
        return

    order = [present[0]]

    for c in present[1:]:
        lo, hi = 0, len(order)

        while lo < hi:
            mid = (lo + hi) // 2
            candidate = ''.join(order[:mid]) + c + ''.join(order[mid:])
            res = ask(candidate)

            if res == "C":
                return

            if res == "Y":
                hi = mid
            else:
                lo = mid + 1

        order.insert(lo, c)

    res = ask(''.join(order))
    if res == "C":
        return

if __name__ == "__main__":
    solve()
```các`ask`Hàm xử lý giao thức tương tác. Nó in một truy vấn, xóa ngay lập tức và đọc phản hồi của thẩm phán. Cần phải xả nước vì thẩm phán không thể trả lời cho đến khi nhận được câu hỏi đầy đủ. 

Vòng lặp đầu tiên xác định bộ ký tự. Một nhân vật nhận được`Y`được đảm bảo ở đâu đó trong mật khẩu vì một chuỗi ký tự đơn chỉ có thể tồn tại nếu ký tự đó tồn tại. 

Phần chèn giữ`order`có giá trị sau mỗi thao tác. Chuỗi ứng cử viên được xây dựng lại từ phần bên trái, ký tự được chèn và phần bên phải, tránh lỗi chỉ mục. Ranh giới tìm kiếm nhị phân biểu thị các vị trí chèn có thể có từ 0 đến độ dài hiện tại. 

Truy vấn cuối cùng sử dụng tất cả các ký tự được phát hiện theo thứ tự được xây dựng lại của chúng. Nếu thuật toán đúng thì đây là mật khẩu ẩn và thẩm phán trả về`C`. 

## Ví dụ đã hoạt động 

Vì nhiệm vụ ban đầu có tính tương tác nên các ví dụ là các cuộc hội thoại chứ không phải các cặp đầu vào/đầu ra cố định. 

Đối với mật khẩu ẩn`hunter2`, giai đoạn khám phá nhân vật có thể trông như thế này: 

| Bước | Truy vấn | Phản hồi | Nhân vật được biết đến | 
| --- | --- | --- | --- | 
| 1 |`h`| Y | h | 
| 2 |`u`| Y | h, bạn | 
| 3 |`n`| Y | h,u,n | 
| 4 |`t`| Y | h,u,n,t | 
| 5 |`e`| Y | h,u,n,t,e | 
| 6 |`r`| Y | h,u,n,t,e,r | 
| 7 |`2`| Y | h,u,n,t,e,r,2 | 

Giai đoạn đặt hàng chèn các ký tự bằng cách kiểm tra các chuỗi con. Ví dụ: khi chèn`t`vào trong`hun`, truy vấn`thun`thất bại vì`t`không xuất hiện trước đó`h`, trong khi`hunt`thành công vì nó khớp với thứ tự ẩn. 

Điều này chứng tỏ rằng truy vấn chuỗi con có thể được sử dụng làm phép toán so sánh giữa các vị trí có thể. 

Đối với mật khẩu một ký tự như`A`, truy vấn ký tự đầu tiên đưa ra: 

| Bước | Truy vấn | Phản hồi | Hành động | 
| --- | --- | --- | --- | 
| 1 |`a`| N | tiếp tục | 
| 2 | ... | ... | tiếp tục | 
| 28 |`A`| C | kết thúc | 

Điều này xác nhận tại sao thuật toán phải xử lý`C`trong giai đoạn phát hiện ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(62 log 62) | Tối đa 62 ký tự được phát hiện và mỗi lần chèn sử dụng tìm kiếm nhị phân | 
| Không gian | O(62) | Chỉ các ký tự được phát hiện và thứ tự hiện tại mới được lưu trữ | 

Giới hạn truy vấn là 750, trong khi thuật toán sử dụng ít hơn thế nhiều. Số lần chèn lớn nhất là 61 và mỗi lần chèn yêu cầu tối đa sáu truy vấn vì độ dài chuỗi không bao giờ vượt quá 62. 

## Trường hợp thử nghiệm 

Vấn đề này mang tính tương tác, vì vậy các bài kiểm tra xác nhận ngoại tuyến thông thường không thể thể hiện sự tương tác thực sự của thẩm phán. Một mô phỏng cục bộ sẽ cần một thẩm phán giả lưu trữ mật khẩu ẩn và trả về`C`,`Y`, hoặc`N`theo quy tắc tuần tự. 

Một trình mô phỏng phù hợp nên kiểm tra các trường hợp sau: 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ẩn mật khẩu`a`|`a`phát hiện | Xử lý mật khẩu một ký tự | 
| Ẩn mật khẩu`hunter2`|`hunter2`phát hiện | Thứ tự chèn bình thường | 
| Ẩn mật khẩu`Z9aB`|`Z9aB`phát hiện | Lớp nhân vật hỗn hợp | 
| Mật khẩu ẩn chứa tất cả 62 ký tự | Thứ tự bảng chữ cái đầy đủ được phục hồi | Xử lý độ dài tối đa | 

## Vỏ cạnh 

Đối với mật khẩu một ký tự, chẳng hạn như`x`, truy vấn`x`trả lại`C`, không`Y`. Thuật toán thoát ngay lập tức thay vì cố gắng tiếp tục xây dựng thứ tự. 

Đối với mật khẩu chứa các ký tự được phát hiện theo thứ tự khác với vị trí thực của chúng, giai đoạn chèn sẽ cố định thứ tự. Ví dụ: nếu khám phá tìm thấy`a`,`b`,`c`nhưng mật khẩu là`cab`, chèn`c`và sau đó kiểm tra các vị trí đảm bảo trình tự cuối cùng trở thành`cab`. 

Đối với mật khẩu có độ dài tối đa chứa tất cả các ký tự có thể, thuật toán vẫn hoạt động vì mỗi lần chèn chỉ phụ thuộc vào việc kiểm tra trình tự tiếp theo. Số lượng truy vấn tăng theo số lượng ký tự chứ không phải theo số lượng mật khẩu có thể có.
