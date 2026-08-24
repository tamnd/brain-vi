---
title: "CF 104295D - \u0421\u0435\u043c\u044c\u044f \u041c\u044e\u043c\u043b\u044b"
description: "Chúng ta được cung cấp một tập hợp cố định các tên họ hiện có, mỗi tên được viết dưới dạng một chuỗi các chữ cái viết thường và dấu gạch nối. Sau đó, chúng tôi được đưa ra một số tên ứng cử viên cho một đứa trẻ sơ sinh. Đối với mỗi ứng cử viên, chúng ta phải quyết định xem liệu điều đó có được chấp nhận hay không. Một ứng viên bị từ chối trong hai trường hợp."
date: "2026-07-01T20:19:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104295
codeforces_index: "D"
codeforces_contest_name: "vkoshp.letovo"
rating: 0
weight: 104295
solve_time_s: 51
verified: true
draft: false
---

[CF 104295D - \u0421\u0435\u043c\u044c\u044f \u041c\u044e\u043c\u043b\u044b](https://codeforces.com/problemset/problem/104295/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp cố định các họ hiện có, mỗi họ được viết dưới dạng một chuỗi các chữ cái viết thường và dấu gạch nối. Sau đó, chúng tôi được đưa ra một số tên ứng cử viên cho một đứa trẻ sơ sinh. Đối với mỗi ứng cử viên, chúng ta phải quyết định xem liệu điều đó có được chấp nhận hay không. 

Một ứng viên bị từ chối trong hai trường hợp. Đầu tiên, nếu nó đã có sẵn trong số các tên hiện có thì nó không thể được sử dụng lại. Thứ hai, ngay cả khi nó là mới, nó vẫn không hợp lệ nếu nó “kết thúc giống” bất kỳ tên hiện có nào, theo nghĩa được mô tả bởi câu lệnh: chúng ta xem xét cấu trúc hậu tố trong đó việc so khớp bỏ qua một tiền tố tùy ý nhưng tôn trọng cấu trúc bên trong của các phần được phân tách bằng dấu gạch nối. Trong thực tế, điều này giúp kiểm tra xem một tên có thể được coi là mẫu hậu tố của tên khác khi căn chỉnh trên dấu gạch ngang hay không, nghĩa là chuỗi kết thúc của các phân đoạn khớp chính xác. 

Một cách hữu ích để diễn đạt lại điều kiện là chúng ta quan tâm đến hậu tố ở cấp độ ký tự, nhưng với hạn chế là dấu gạch nối là một phần của cấu trúc. Vì vậy, hai tên được coi là trùng khớp ở cuối nếu chúng ta có thể loại bỏ một số tiền tố (có thể trống) khỏi cả hai chuỗi theo cách phù hợp với định nghĩa, để lại các chuỗi còn lại giống hệt nhau. 

Các ràng buộc rất lớn: tối đa 10.000 tên hiện có và 10.000 truy vấn, mỗi truy vấn có độ dài tối đa 50. Một so sánh đơn giản giữa mỗi truy vấn với mọi tên được lưu trữ với các kiểm tra chuỗi đầy đủ sẽ yêu cầu tới 100 triệu so sánh, mỗi truy vấn có tối đa 50 ký tự, vẫn ở ranh giới nhưng sẽ trở nên rủi ro nếu chúng ta thực hiện các thao tác chuỗi con lặp lại hoặc xây dựng lại các chuỗi cho mỗi lần kiểm tra. Quan trọng hơn, việc kết hợp hậu tố ngây thơ trên mỗi cặp có thể chuyển thành quá trình quét lặp đi lặp lại. 

Vấn đề cấu trúc quan trọng là chúng ta không so sánh các chuỗi con tùy ý mà kiểm tra tư cách thành viên của các chuỗi đầy đủ và hậu tố của chúng. Điều này gợi ý rõ ràng về một cấu trúc tiền xử lý giống như một bộ băm cho các chuỗi đầy đủ và một bộ khác cho tất cả các hậu tố tương ứng với “kết thúc họ” hợp lệ. 

Một trường hợp cạnh tinh tế phát sinh khi một ứng cử viên bằng chính xác một tên hiện có. Điều đó phải bị từ chối ngay lập tức mặc dù nó cũng phù hợp với điều kiện hậu tố của chính nó. Một trường hợp đặc biệt khác là khi hai tên có cùng hậu tố nhưng khác nhau về tiền tố, ví dụ “anna-katerina-mymla” và “mymla”. Tên ứng cử viên bằng “mymla” là không hợp lệ và bất kỳ tên nào dài hơn kết thúc bằng “mymla” cũng không hợp lệ. 

Cuối cùng, tên tương đối ngắn (≤ 50), do đó việc tạo tất cả các hậu tố cho mỗi tên là khả thi, nhưng chúng ta phải đảm bảo rằng chúng ta tạo chúng theo cách được kiểm soát thay vì cắt các chuỗi liên tục bên trong các vòng lặp lồng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi tên truy vấn, chúng tôi so sánh nó với mọi tên hiện có. Trước tiên, chúng tôi kiểm tra sự bằng nhau, sau đó kiểm tra xem truy vấn có khớp với điều kiện hậu tố với tên hiện có đó hay không. Kiểm tra hậu tố trực tiếp yêu cầu quét từ cuối cả hai chuỗi, có khả năng căn chỉnh sau khi cắt nhất quán với dấu phân cách. Mỗi so sánh có chi phí O(L) trong đó L ≤ 50, do đó tổng chi phí trở thành O(nqL), trong trường hợp xấu nhất là 10.000 × 10.000 × 50, quá lớn. 

Quan sát quan trọng là tính không hợp lệ của hậu tố chỉ phụ thuộc vào việc truy vấn có xuất hiện dưới dạng mẫu hậu tố hợp lệ của bất kỳ tên hiện có nào hay không. Thay vì tính toán lại kết quả này cho mọi truy vấn, chúng ta có thể xử lý trước tất cả hậu tố của các tên hiện có và lưu trữ chúng trong tập hợp băm. Sau đó, mỗi truy vấn giảm xuống còn hai lần kiểm tra liên tục: tư cách thành viên đầy đủ trong tập hợp hiện có và tư cách thành viên trong tập hậu tố. 

Chúng tôi tạo hậu tố bằng cách lặp lại từng vị trí trong tên và lấy chuỗi con bắt đầu từ đó. Vì tên ngắn nên tồn tại tối đa 50 hậu tố cho mỗi tên, do đó tổng số tiền xử lý là khoảng 500.000 chuỗi con, nằm trong giới hạn. 

Điều này biến vấn đề thành một vấn đề truy vấn thành viên trên hai bộ.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nqL) | O(1) | Quá chậm | 
| Tối ưu (bộ băm tên và hậu tố) | O((n + q)L) | O(nL) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai bộ băm, một cho tất cả các tên hiện có và một cho tất cả các hậu tố bắt nguồn từ chúng. 

1. Đọc tất cả các tên hiện có và chèn từng tên đầy đủ vào một tập hợp có tên`names`. Điều này cho phép kiểm tra sự bình đẳng theo thời gian liên tục để từ chối. 
2. Đối với mỗi tên hiện có, hãy tạo tất cả các hậu tố bằng cách lấy các chuỗi con bắt đầu từ mọi chỉ mục đến cuối và chèn từng hậu tố vào bộ thứ hai được gọi là`suffixes`. Điều này nắm bắt mọi mẫu kết thúc có thể có sẽ làm mất hiệu lực của một ứng cử viên. 
3. Đối với mỗi tên truy vấn, trước tiên hãy kiểm tra xem nó có tồn tại trong`names`. Nếu đúng như vậy, hãy xuất ra “Xấu” ngay lập tức vì việc sao chép bị cấm. 
4. Ngược lại, hãy kiểm tra xem truy vấn có tồn tại trong`suffixes`. Nếu đúng như vậy, hãy xuất ra “Xấu” vì nó khớp với kiểu kết thúc của một thành viên gia đình hiện có. 
5. Nếu không có điều kiện nào xảy ra, xuất ra “Tốt”. 

Thứ tự quan trọng: sự bình đẳng phải được kiểm tra trước tiên, vì sự bình đẳng cũng bao hàm tư cách thành viên hậu tố nhưng thể hiện điều kiện không hợp lệ mạnh hơn. 

### Tại sao nó hoạt động 

Bộ hậu tố chứa chính xác tất cả các chuỗi có thể xuất hiện dưới dạng phần cuối hợp lệ của một số tên hiện có. Bất kỳ ứng cử viên không hợp lệ nào cũng phải giống với tên hiện có hoặc xuất hiện dưới dạng một trong những hậu tố này. Bởi vì việc tạo hậu tố liệt kê tất cả các chuỗi con đuôi có thể có nên không có mẫu không hợp lệ nào bị bỏ sót. Ngược lại, bất kỳ chuỗi nào trong bộ hậu tố đều tương ứng với hậu tố thực tế của một số tên hiện có, do đó việc đánh dấu nó không hợp lệ là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    names = set()
    suffixes = set()

    for _ in range(n):
        s = input().strip()
        names.add(s)
        L = len(s)
        for i in range(L):
            suffixes.add(s[i:])

    q = int(input())
    out = []

    for _ in range(q):
        t = input().strip()
        if t in names:
            out.append("Bad")
        elif t in suffixes:
            out.append("Bad")
        else:
            out.append("Good")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng hai bộ để tách các kết quả khớp chính xác khỏi sự vô hiệu hóa dựa trên hậu tố. Vòng lặp tạo hậu tố là phần duy nhất nặng về tiền xử lý, nhưng nó vẫn tuyến tính về tổng chiều dài chuỗi trên tất cả các đầu vào. Mỗi truy vấn được giảm xuống còn hai lần tra cứu băm. 

Một điểm tinh tế là chúng tôi lưu trữ trực tiếp các chuỗi hậu tố thô. Vì độ dài chuỗi tối đa là 50 nên chi phí cắt bị giới hạn và không ảnh hưởng đến hiệu suất tiệm cận. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ:```
3
anna-katerina-mymla
mymla
snus-mumrik
3
mymla
anna-mymla
katerina-mymla
```Chúng tôi xây dựng: 

| Tên | Chèn vào tên | Hậu tố được tạo | 
| --- | --- | --- | 
| anna-katerina-mymla | vâng | katerina-mymla, mymla, ... | 
| mymla | vâng | mymla, ... | 
| snus-mumrik | vâng | mẹ, ... | 

Bây giờ truy vấn: 

| Truy vấn | Trong tên | Trong hậu tố | Kết quả | 
| --- | --- | --- | --- | 
| mymla | vâng | vâng | Xấu | 
| anna-mymla | không | không | Tốt | 
| katerina-mymla | không | vâng | Xấu | 

Truy vấn đầu tiên bị từ chối do sự bình đẳng. Mẫu thứ hai vượt qua vì nó không khớp với bất kỳ mẫu hậu tố đầy đủ nào. Tên thứ ba bị từ chối vì nó khớp với hậu tố của tên đầu tiên. 

Điều này chứng tỏ rằng việc vô hiệu hóa dựa trên hậu tố không phụ thuộc vào việc bản thân truy vấn có phải là tên đầy đủ trong cơ sở dữ liệu hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nL + q) | Mỗi tên đóng góp việc tạo hậu tố O(L), mỗi truy vấn là tra cứu tập hợp trung bình O(1) | 
| Không gian | O(nL) | Tất cả các hậu tố cộng với tên đầy đủ được lưu trữ trong bộ băm | 

Các ràng buộc cho phép tối đa 10.000 tên có độ dài lên tới 50, do đó việc lưu trữ khoảng 500.000 chuỗi hậu tố là có thể chấp nhận được. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample (adapted format if needed)
assert run("""3
anna-katerina-mymla
mymla
snus-mumrik
6
mymla
my
anna-mymla
katerina-mymla
anna-katerina
snu-snusmumrik
""") == """Bad
Good
Good
Bad
Good
Good"""

# minimum case
assert run("""1
a
2
a
b
""") == """Bad
Good"""

# no suffix collisions
assert run("""2
abc
def
2
x
y
""") == """Good
Good"""

# all queries invalid via suffix
assert run("""1
abcde
3
abcde
bcde
cde
""") == """Bad
Bad
Bad"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sao chép char đơn | Xấu/Tốt | bình đẳng vs không tồn tại | 
| tên rời rạc | Tốt/Tốt | không có hậu tố phù hợp | 
| hậu tố xếp tầng | tất cả đều tệ | truyền bá hậu tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi truy vấn hoàn toàn giống với tên hiện có. Đối với đầu vào:```
1
mymla-my
1
mymla-my
```Thuật toán chèn tên vào`names`, sau đó kiểm tra truy vấn. Điều kiện đầu tiên`t in names`là đúng, do đó kết quả đầu ra ngay lập tức là “Xấu”. Mặc dù nó cũng nằm trong tập hậu tố, nhưng việc kiểm tra đẳng thức sớm sẽ phân loại nó một cách chính xác. 

Một trường hợp khác là truy vấn không phải tên đầy đủ mà là hậu tố của nhiều tên khác nhau:```
2
a-b-c
x-b-c
1
b-c
```Cả hai tên hiện có đều tạo ra hậu tố “b-c”. Bộ hậu tố chứa nó một lần, nhưng thế là đủ. Truy vấn không có trong`names`, nhưng nó ở trong`suffixes`, nên nó bị từ chối. Điều này cho thấy rằng các bản sao trên các nguồn khác nhau không thành vấn đề vì các bộ sẽ tự động thu gọn chúng.
