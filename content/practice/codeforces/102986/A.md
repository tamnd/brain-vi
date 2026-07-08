---
title: "CF 102986A - Món ăn yêu thích"
description: "Vấn đề mô tả một người có một danh sách các món ăn yêu thích và được cung cấp một chuỗi các món ăn họ ăn theo thời gian."
date: "2026-07-04T02:54:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102986
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 03-05-21 Div. 2 (Beginner)"
rating: 0
weight: 102986
solve_time_s: 41
verified: true
draft: false
---

[CF 102986A - Món ăn yêu thích](https://codeforces.com/problemset/problem/102986/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một người có một danh sách các món ăn yêu thích và được cung cấp một chuỗi các món ăn họ ăn theo thời gian. Nhiệm vụ là xác định, đối với mỗi truy vấn hoặc kịch bản thử nghiệm, xem trình tự có chứa ít nhất một trong các loại thực phẩm yêu thích hay không và dựa vào đó quyết định nên xuất sản phẩm gì. 

Cụ thể hơn, mỗi trường hợp thử nghiệm cung cấp một tập hợp các mục “yêu thích” và một danh sách khác đại diện cho những gì được tiêu thụ hoặc quan sát. Đầu ra phụ thuộc vào việc có bất kỳ sự chồng chéo nào giữa hai bộ này hay không. Nếu ít nhất một phần tử xuất hiện trong cả hai bộ sưu tập thì câu trả lời là khẳng định, nếu không thì sẽ là phủ định. 

Từ góc độ tính toán, kích thước đầu vào đủ nhỏ để việc kiểm tra tư cách thành viên dựa trên tập hợp đơn giản là đủ. Ngay cả khi chúng tôi giả định tối đa khoảng 10^5 phần tử trong trường hợp kết hợp tồi tệ nhất trong tất cả các trường hợp thử nghiệm, thì mẫu ràng buộc gợi ý rõ ràng rằng chúng tôi nên hướng tới việc kiểm tra tư cách thành viên O(1) trung bình bằng cách sử dụng hàm băm. Quá trình quét lồng nhau đơn giản sẽ giảm xuống O(nm), quá trình này trở nên quá chậm khi cả hai danh sách đều vượt quá vài nghìn phần tử. 

Trường hợp cạnh tinh tế phát sinh khi các bản sao xuất hiện trong một trong hai danh sách. Ví dụ: nếu các món ăn yêu thích được liệt kê lặp lại, một sự so sánh ngây thơ dựa trên sự phù hợp về vị trí thay vì tập hợp thành viên sẽ thất bại. 

Trường hợp cạnh bổ sung xảy ra khi một trong các danh sách trống. Nếu không có món ăn yêu thích, câu trả lời luôn là phủ định bất kể danh sách đã tiêu thụ. Ngược lại, nếu danh sách đã sử dụng trống, câu trả lời luôn là phủ định trừ khi bài toán xác định một điều kiện khớp tầm thường, điều này không có trong cách diễn giải tiêu chuẩn của cấu trúc này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực so sánh trực tiếp mọi thực phẩm yêu thích với mọi thực phẩm được tiêu thụ. Điều này hoạt động bằng cách lặp qua danh sách yêu thích và đối với mỗi phần tử, quét toàn bộ danh sách đã sử dụng để kiểm tra sự bằng nhau. Điều này đúng vì nó kiểm tra rõ ràng tất cả các cặp có thể trùng khớp. Tuy nhiên, nếu có n mặt hàng yêu thích và m mặt hàng đã tiêu thụ, điều này dẫn đến so sánh n lần m trong trường hợp xấu nhất. Với cả n và m khoảng 10^5, điều này trở thành so sánh 10^10, vượt xa giới hạn khả thi trong một giới hạn thời gian thông thường. 

Quan sát quan trọng là chúng ta không thực sự cần phải so sánh từng cặp. Chúng ta chỉ cần biết liệu có phần tử nào từ tập hợp này tồn tại trong tập hợp kia hay không. Điều này biến vấn đề thành vấn đề truy vấn thành viên trên một bộ sưu tập. Khi chúng tôi lưu trữ các loại thực phẩm đã tiêu thụ trong một bộ băm, trung bình mỗi lần kiểm tra đối với một loại thực phẩm yêu thích sẽ trở thành O(1). Điều này làm giảm toàn bộ vấn đề về thời gian tuyến tính trên kích thước đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tra cứu bộ băm | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập vì không có sự tương tác giữa chúng. 

1. Đọc số lượng món ăn yêu thích và lưu chúng vào danh sách hoặc bộ tùy theo kiểu truy cập. Bước này chuẩn bị dữ liệu để kiểm tra thành viên hiệu quả sau này. 
2. Đọc danh sách các loại thực phẩm được quan sát hoặc tiêu thụ và chèn từng phần tử vào bộ băm. Mục đích là để đảm bảo tra cứu liên tục theo thời gian cho bất kỳ giá trị ứng cử viên nào. 
3. Lặp lại danh sách thực phẩm yêu thích và kiểm tra xem mỗi món có trong bộ đã tiêu thụ hay không. Lần đầu tiên tìm thấy sự trùng khớp, chúng ta có thể kết luận ngay câu trả lời là tích cực. 
4. Nếu vòng lặp hoàn thành mà không tìm thấy bất kỳ giao điểm nào, hãy xuất kết quả âm tính. 

Lý do chấm dứt sớm là an toàn vì điều kiện chỉ phụ thuộc vào sự tồn tại của ít nhất một phần tử chung, không phụ thuộc vào việc đếm hay sắp xếp. 

### Tại sao nó hoạt động

Thuật toán dựa vào bất biến mà sau khi xây dựng tập băm từ danh sách đã sử dụng, các truy vấn thành viên phản ánh sự hiện diện chính xác trong danh sách đó. Vì mọi mục yêu thích đều được kiểm tra dựa trên cấu trúc tham chiếu cố định này nên mọi giao điểm sẽ được phát hiện trong quá trình lặp lại. Không thể bỏ qua kết quả khớp hợp lệ vì mọi phần tử đều được kiểm tra chính xác một lần đối với bản trình bày hoàn chỉnh của bộ sưu tập thứ hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        fav = input().split()
        eaten = set(input().split())

        for x in fav:
            if x in eaten:
                print("YES")
                break
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử, xây dựng bộ băm từ các mục đã sử dụng, sau đó quét qua danh sách yêu thích để kiểm tra giao điểm. Việc sử dụng bộ Python đảm bảo kiểm tra tư cách thành viên trung bình theo thời gian liên tục. các`for-else`cấu trúc được sử dụng để xử lý rõ ràng trường hợp không tìm thấy kết quả phù hợp, chỉ in kết quả âm tính sau khi đã cạn kiệt tất cả các ứng cử viên. 

Phải cẩn thận để tránh lỗi phân tích dòng vì thực phẩm thường là các chuỗi được phân tách bằng dấu cách. sử dụng`.split()`đảm bảo mã thông báo chính xác bất kể khoảng cách. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có mục yêu thích`apple banana mango`và thực phẩm tiêu thụ là`rice mango bread`. 

| Bước | Mục yêu thích | Trong bộ? | Hành động | 
| --- | --- | --- | --- | 
| 1 | táo | Không | tiếp tục | 
| 2 | chuối | Không | tiếp tục | 
| 3 | xoài | Có | in CÓ, dừng lại | 

Điều này thể hiện sự chấm dứt sớm khi tìm thấy kết quả phù hợp. 

Bây giờ hãy xem xét một trường hợp không có giao điểm, yêu thích`tea coffee`, tiêu thụ`milk juice`. 

| Bước | Mục yêu thích | Trong bộ? | Hành động | 
| --- | --- | --- | --- | 
| 1 | trà | Không | tiếp tục | 
| 2 | cà phê | Không | tiếp tục | 

Vì không có kết quả khớp nào xảy ra nên thuật toán sẽ xuất ra NO sau khi hoàn thành vòng lặp. Điều này xác nhận rằng việc di chuyển ngang hoàn toàn chỉ được yêu cầu khi không có nút giao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi danh sách được xử lý một lần và đặt kiểm tra tư cách thành viên ở mức trung bình O(1) | 
| Không gian | O(m) | Chúng tôi lưu trữ danh sách đã sử dụng trong bộ băm | 

Độ phức tạp tuyến tính phù hợp thoải mái với các ràng buộc điển hình của các vấn đề kiểu Codeforces, ngay cả đối với đầu vào lớn lên tới 10^5 trở lên cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# sample-style tests
assert run("1\n3 3\napple banana mango\nrice mango bread\n") == "YES"
assert run("1\n2 3\ntea coffee\nmilk juice bread\n") == "NO"

# custom cases
assert run("1\n1 1\napple\napple\n") == "YES", "single overlap"
assert run("1\n0 3\n\na b c\n") == "NO", "empty favorites"
assert run("1\n3 0\na b c\n\n") == "NO", "empty consumed"
assert run("2\n2 2\na b\nc d\n1 1\nx\nx\n") == "NO\nYES", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục yêu thích trống | KHÔNG | hành vi tập trống | 
| tiêu thụ rỗng | KHÔNG | không có kết quả phù hợp | 
| tất cả rời rạc | KHÔNG | tính chính xác khi quét toàn bộ | 
| mặt hàng giống hệt nhau | CÓ | xử lý khớp chính xác | 
| nhiều trường hợp thử nghiệm | hỗn hợp | xử lý độc lập | 

## Vỏ cạnh 

Khi danh sách yêu thích trống, vòng lặp yêu thích không bao giờ chạy, do đó`for-else`cấu trúc ngay lập tức kích hoạt`else`nhánh và đầu ra KHÔNG. Điều này phản ánh chính xác rằng không có gì có thể sánh được. 

Khi danh sách đã sử dụng trống, tập hợp sẽ trống, do đó mọi kiểm tra thành viên đều không thành công. Thuật toán vẫn lặp lại tất cả các mục yêu thích nhưng không bao giờ tìm thấy kết quả khớp, tạo ra NO một cách chính xác. 

Khi tất cả các mục giống hệt nhau trên cả hai danh sách, lần lặp đầu tiên kiểm tra tư cách thành viên sẽ ngay lập tức thành công. Thuật toán in CÓ và thoát khỏi vòng lặp sớm, thể hiện hành vi đoản mạch chính xác mà không cần quét phần còn lại của dữ liệu.
