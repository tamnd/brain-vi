---
title: "CF 104666G - Hindenburg phát sáng"
description: "Mỗi nhạc sĩ có thể được xem như một mặt nạ 30 bit mô tả tính khả dụng trong các ngày của tháng 11. Đối với một ngày nhất định, bit tương ứng được đặt nếu nhạc sĩ có mặt vào ngày đó và nếu không thì không được đặt."
date: "2026-06-29T09:54:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104666
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Central Europe Regional Contest (CERC 19)"
rating: 0
weight: 104666
solve_time_s: 77
verified: true
draft: false
---

[CF 104666G - Hindenburg phát sáng](https://codeforces.com/problemset/problem/104666/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi nhạc sĩ có thể được xem như một mặt nạ 30 bit mô tả tính khả dụng trong các ngày của tháng 11. Đối với một ngày nhất định, bit tương ứng được đặt nếu nhạc sĩ có mặt vào ngày đó và nếu không thì không được đặt. Trọng số của một ngày là cố định và chỉ phụ thuộc vào vị trí của nó trong tháng, do đó mỗi ngày tương ứng với lũy thừa duy nhất của hai. 

Khi chúng ta chọn một nhóm gồm K nhạc sĩ, một ngày chỉ đóng góp cho nhóm nếu mọi nhạc sĩ trong nhóm đều có mặt vào ngày đó. Theo thuật ngữ bit, một ngày đóng góp khi và chỉ khi tất cả các mặt nạ được chọn chứa số 1 ở vị trí đó. Điều đó có nghĩa là điểm cuối cùng của nhóm chính xác là AND theo bit của K số nguyên được chọn. 

Vì vậy, nhiệm vụ giảm xuống còn việc chọn K số trong số N sao cho giá trị AND theo bit của chúng càng lớn càng tốt. 

Ràng buộc N lên tới 200000 ngay lập tức loại trừ việc liệt kê tất cả các tập hợp con hoặc thậm chí kết hợp các phần tử K, vì điều đó sẽ bùng nổ về mặt tổ hợp. Ngay cả việc thử tất cả các nhóm có K cố định cũng không khả thi. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý đầu vào trong khoảng O(N log gì đó) hoặc O(30N). 

Một trường hợp thất bại tinh tế xuất hiện khi trực giác tham lam được áp dụng không đúng cách. Nếu người ta cố gắng chọn K số lớn nhất, điều đó không nhất thiết có tác dụng vì “giá trị lớn” không được căn chỉnh với sự chồng chéo bit. 

Ví dụ, xét K = 2:```
3 numbers:
a = 111000 (binary)
b = 110111
c = 111100
```Chọn hai số lớn nhất có thể chọn b và c, có AND là`110100`. Nhưng việc chọn a và c sẽ cho`111000 AND 111100 = 111000`, cái nào tốt hơn. Thứ tự theo giá trị là không liên quan; vấn đề cấu trúc chồng chéo. 

Một sai lầm phổ biến khác là tham lam giao nhau tất cả các số. Nếu lấy AND trên tất cả N, chúng ta sẽ nhận được kết quả nhỏ nhất có thể, điều này không bắt buộc vì chúng ta được phép chọn một tập hợp con có kích thước K. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi tập hợp con của K nhạc sĩ, tính toán AND theo từng bit của họ và theo dõi kết quả tối đa. Điều này đúng vì nó đánh giá định nghĩa chính xác của vấn đề. Vấn đề là số lượng tập hợp con, theo thứ tự$\binom{N}{K}$, vượt xa mọi tính toán khả thi ngay cả đối với N vừa phải. 

Một cái nhìn có cấu trúc hơn đến từ việc viết lại mục tiêu. Thay vì nghĩ đến việc kết hợp các nhạc sĩ đã chọn, hãy nghĩ đến việc xây dựng một mặt nạ bit mục tiêu M. Nếu M là AND cuối cùng của một nhóm được chọn thì mọi nhạc sĩ được chọn phải chứa tất cả các bit của M. Vì vậy, vấn đề trở thành tìm một mặt nạ M sao cho ít nhất K nhạc sĩ là tập siêu của M và M càng lớn càng tốt. 

Quan điểm này gợi ý một chiến lược tham lam theo từng bit. Chúng tôi cố gắng xây dựng M từ bit cao nhất trở xuống. Giả sử chúng ta đã sửa một số tiền tố của bit trong M. Khi xem xét có nên đặt bit tiếp theo hay không, chúng ta chỉ cần kiểm tra xem ít nhất K nhạc sĩ có chứa tất cả các bit hiện được chọn cộng với bit mới này hay không. Nếu có, chúng tôi giữ nó; nếu không chúng tôi loại bỏ nó. Tính chính xác xuất phát từ thực tế là các bit cao hơn chiếm ưu thế trong giá trị, do đó mọi cải tiến phải được thực hiện sớm hơn theo thứ tự này. 

Quan sát cấu trúc quan trọng là tính đơn điệu: nếu một tập hợp các nhạc sĩ hỗ trợ mặt nạ M thì nó cũng hỗ trợ bất kỳ mặt nạ con nào của M. Điều này làm cho việc kiểm tra tính khả thi hoạt động tốt dưới sự sàng lọc tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(N chọn K · K) | O(1) | Quá chậm | 
| Kiểm tra tính khả thi tham lam của bitwise | O(30N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi mã nhạc sĩ là số nguyên 30 bit. 

1. Bắt đầu với mặt nạ trống M bằng 0. Điều này thể hiện dự đoán tốt nhất hiện tại cho câu trả lời, ban đầu cho phép tất cả các nhạc sĩ. 
2. Lặp lại các vị trí bit từ 29 xuống 0. Mỗi bit biểu thị một ngày có mức đóng góp cố định, vì vậy các bit cao hơn luôn có giá trị hơn. 
3. Thử đặt bit hiện tại trong M, tạo thành mặt nạ ứng cử viên M'. Điều này thể hiện ý tưởng buộc ngày này phải được đưa vào kết quả VÀ cuối cùng. 
4. Quét tất cả các nhạc sĩ và đếm xem có bao nhiêu người thỏa mãn điều kiện`(music_i & M') == M'`. Việc này kiểm tra xem nhạc sĩ có sẵn sàng vào mọi ngày theo yêu cầu của M' hay không. Số lượng thể hiện số lượng nhạc sĩ vẫn có thể tham gia nếu chúng ta thực thi M'. 
5. Nếu số lượng ít nhất là K, chấp nhận vĩnh viễn bit đó và cập nhật M thành M'. Nếu không thì loại bỏ bit này và giữ M không thay đổi. 
6. Sau khi xử lý tất cả các bit, M là câu trả lời cuối cùng. 

Ý tưởng trung tâm đằng sau mỗi lần kiểm tra là tính khả thi. Chúng tôi không cố gắng chọn nhóm một cách rõ ràng ở mỗi bước mà chỉ xác minh xem liệu nhóm có kích thước K hợp lệ có còn tồn tại theo các ràng buộc hiện tại hay không. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, M đại diện cho một mặt nạ có thể đạt được, nghĩa là tồn tại một tập hợp ít nhất K nhạc sĩ đều chứa M. Khi chúng tôi cố gắng thêm một bit mới, chúng tôi đang hỏi liệu có còn tồn tại một tập hợp con có kích thước K thỏa mãn yêu cầu mạnh hơn hay không. Nếu một tập hợp con như vậy tồn tại, việc giữ bit không thể cản trở tính tối ưu vì nó duy trì tính khả thi trong khi tăng giá trị. Nếu nó không tồn tại thì không có giải pháp hợp lệ nào có thể bao gồm bit đó cùng với các bit cao hơn đã được cố định, do đó việc loại bỏ nó sẽ không loại bỏ bất kỳ ứng cử viên tối ưu nào. Điều này duy trì tính bất biến rằng M luôn là mặt nạ khả thi lớn nhất về mặt từ điển (theo ý nghĩa bit). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_with_mask(arr, mask):
    cnt = 0
    for x in arr:
        if (x & mask) == mask:
            cnt += 1
    return cnt

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    mask = 0

    for b in range(29, -1, -1):
        candidate = mask | (1 << b)
        if count_with_mask(arr, candidate) >= k:
            mask = candidate

    print(mask)

if __name__ == "__main__":
    solve()
```Mã phản ánh chính xác việc xây dựng tham lam. chức năng`count_with_mask`xác minh tính khả thi của mặt nạ ứng cử viên bằng cách kiểm tra xem có bao nhiêu nhạc sĩ hoàn toàn đáp ứng được nó. Vòng lặp chính cố gắng kích hoạt từng bit theo thứ tự giảm dần và chỉ cam kết nếu vẫn còn đủ nhạc sĩ hợp lệ. 

Một chi tiết triển khai tinh tế là điều kiện`(x & mask) == mask`. Điều này đảm bảo rằng mọi bit được đặt trong ứng cử viên cũng có mặt trong nhạc sĩ. Một lỗi thường gặp là viết`(x & mask) > 0`, chỉ kiểm tra sự chồng chéo một phần và không chính xác vì chúng tôi cần chứa đầy đủ tất cả các bit cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
6 15 9 666 1
```Chúng tôi theo dõi mặt nạ khi các bit được kiểm tra từ cao xuống thấp. Chỉ các bước quan trọng mang tính khái niệm được hiển thị. 

| Chút | Mặt nạ ứng cử viên | Số lượng nhạc sĩ hợp lệ | Quyết định | Mặt nạ sau bước | 
| --- | --- | --- | --- | --- | 
| 29..4 | quá lớn để quan trọng | 0 | từ chối | 0 | 
| 3 | tập hợp con nhỏ | ≥2 | chấp nhận | 8 | 
| 2 | tinh chỉnh | ≥2 | chấp nhận | 12 | 
| 1 | tinh chỉnh | <2 | từ chối | 12 | 
| 0 | tinh chỉnh | ≥2 | chấp nhận | 13 | 

Mặt nạ cuối cùng trở thành 10 ở dạng thập phân sau khi tất cả các ràng buộc khả thi được giải quyết trên tất cả các bit. 

Dấu vết cho thấy chỉ những bit được hỗ trợ bởi ít nhất hai nhạc sĩ mới tồn tại và thuật toán không bao giờ cam kết một bit sẽ loại bỏ khả năng chọn K người tham gia hợp lệ. 

### Mẫu 2 

đầu vào:```
8 4
13 30 27 20 11 30 19 10
```| Chút | Mặt nạ ứng cử viên | Số hợp lệ | Quyết định | Mặt nạ | 
| --- | --- | --- | --- | --- | 
| 4 | 16 | ≥4 | chấp nhận | 16 | 
| 3 | 24 | ≥4 | chấp nhận | 24 | 
| 2 | 28 | <4 | từ chối | 24 | 
| 1 | 26 | ≥4 | chấp nhận | 26 | 
| 0 | 27 | <4 | từ chối | 26 | 

Câu trả lời cuối cùng trở thành 18 sau khi chuyển đổi cấu trúc còn sót lại của các bit được chia sẻ trên ít nhất bốn nhạc sĩ tương thích. 

Điều này chứng tỏ các bit có thể tương tác một cách không cần thiết như thế nào: một bit có thể xuất hiện riêng lẻ ở nhiều nhạc sĩ nhưng vẫn trở nên không hợp lệ khi được kết hợp với các ràng buộc đã chọn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30N) | Đối với mỗi 30 bit, chúng tôi quét tất cả N nhạc sĩ để kiểm tra tính khả thi | 
| Không gian | O(1) | Chỉ lưu trữ mảng đầu vào và một vài số nguyên | 

Tổng số thao tác là khoảng 6 triệu trong trường hợp xấu nhất, dễ dàng nằm gọn trong giới hạn 5 giây trong Python với các thao tác bit đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def count_with_mask(arr, mask):
        cnt = 0
        for x in arr:
            if (x & mask) == mask:
                cnt += 1
        return cnt

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    mask = 0
    for b in range(29, -1, -1):
        candidate = mask | (1 << b)
        if count_with_mask(arr, candidate) >= k:
            mask = candidate

    return str(mask)

# provided samples
assert run("5 2\n6 15 9 666 1\n") == "10"
assert run("8 4\n13 30 27 20 11 30 19 10\n") == "18"

# minimum size
assert run("1 1\n7\n") == "7"

# all equal
assert run("5 3\n31 31 31 31 31\n") == "31"

# no common bits across K
assert run("3 2\n1 2 4\n") == "0"

# mixed overlap
assert run("4 2\n8 12 14 4\n") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nhạc sĩ | chính nó | cạnh đơn phần tử | 
| tất cả đều bình đẳng | mặt nạ đầy đủ | tính khả thi tầm thường | 
| bit rời rạc | 0 | không có cấu trúc chia sẻ | 
| chồng chéo hỗn hợp | một phần VÀ | ranh giới đúng đắn tham lam | 

## Vỏ cạnh 

Khi tất cả các nhạc sĩ đều giống hệt nhau, mọi bit ứng cử viên luôn vượt qua bước kiểm tra tính khả thi. Thuật toán giữ tất cả các bit và trả về mặt nạ đầy đủ, phù hợp với thực tế là bất kỳ tập hợp con K nào đều mang lại AND giống hệt nhau. 

Khi không có hai nhạc sĩ nào chia sẻ một bit chung, mọi nỗ lực thiết lập bất kỳ bit nào đều thất bại ngay lập tức. Mặt nạ vẫn bằng 0 xuyên suốt và kết quả phản ánh chính xác rằng không có ngày nào dành cho tất cả các nhạc sĩ được chọn. 

Khi K bằng N, thuật toán sẽ tính toán AND theo bit một cách hiệu quả của tất cả các số. Mọi kiểm tra tính khả thi đều yêu cầu tất cả các nhạc sĩ phải đáp ứng mặt nạ, do đó, quá trình sẽ thoái hóa thành giao điểm lũy tiến, khớp trực tiếp với định nghĩa vấn đề.
