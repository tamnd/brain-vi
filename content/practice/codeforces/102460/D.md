---
title: "CF 102460D - Bột sắn"
description: "Tên món ăn có đúng ba từ viết thường. Một số từ đó là một phần của trang trí tapioka và phải được loại bỏ. Các từ có thể tháo rời chính xác là bong bóng và bột sắn."
date: "2026-08-08T10:02:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 194
verified: true
draft: false
---

[CF 102460D - Tapioka](https://codeforces.com/problemset/problem/102460/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tên món ăn có đúng ba từ viết thường. Một số từ đó là một phần của trang trí tapioka và phải được loại bỏ. Những từ có thể tháo rời là chính xác`bubble`Và`tapioka`. Mọi từ khác đều thuộc về món ăn thực tế và phải giữ nguyên vị trí ban đầu so với các từ còn lại. 

Ví dụ,`bubble tea pizza`trở thành`tea pizza`, trong khi`tapioka cake tapiokas`trở thành`cake tapiokas`. Ví dụ thứ hai hữu ích một cách có chủ ý vì`tapiokas`không phải là từ giống như`tapioka`, vì vậy nó không được loại bỏ. 

Sau khi lọc cả ba từ, có thể không còn gì. Trong trường hợp đó, đầu ra được yêu cầu là từ`nothing`. 

Đầu vào cực kỳ nhỏ: có chính xác ba từ và mỗi từ có độ dài tối đa là 32. Ngay cả một thuật toán quét liên tục toàn bộ đầu vào cũng sẽ chỉ thực hiện một số thao tác không đổi. Do đó, giới hạn thời gian 2 giây và giới hạn bộ nhớ lớn không hạn chế đối với vấn đề này. Tổng quát hơn, nếu cùng một tác vụ được mở rộng thành một mảng có tối đa (10^5) từ thì giải pháp (O(n)) sẽ là mục tiêu đương nhiên, trong khi việc quét liên tục toàn bộ mảng sau mỗi lần xóa có thể trở thành (O(n^2)). 

Có một số trường hợp việc thực hiện bất cẩn có thể dẫn đến kết quả sai. Đầu tiên, một từ phải khớp chính xác. Vì`tapioka cake tapiokas`, đầu ra đúng là`cake tapiokas`. Sự thay thế dựa trên chuỗi con có thể xử lý không chính xác`tapiokas`như chứa từ có thể tháo rời`tapioka`, thao tác này sẽ xóa một từ còn sót lại. 

Thứ hai, tất cả các từ có thể di chuyển được có thể biến mất. Vì`tapioka bubble tapioka`, mỗi từ đều có thể xóa được nên kết quả đúng là`nothing`. Thay vào đó, một giải pháp chỉ cần nối danh sách còn lại sẽ tạo ra một dòng trống. 

Thứ ba, thứ tự các từ còn sót lại không được thay đổi. Vì`bubble ramen cake`, đầu ra đúng là`ramen cake`. Lọc tại chỗ hoặc xây dựng danh sách mới đều có tác dụng nhưng việc sắp xếp hoặc sắp xếp lại các từ sẽ vi phạm thứ tự bắt buộc. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo trực tiếp là liên tục xem qua danh sách hiện tại, loại bỏ mọi từ bằng`bubble`hoặc`tapioka`và tiếp tục cho đến khi không còn từ nào có thể tháo rời được. Điều này đúng vì mọi lần xuất hiện cuối cùng đều bị xóa, trong khi mọi từ khác vẫn được giữ nguyên. Với chính xác ba từ, trường hợp xấu nhất thực hiện tối đa ba lần quét toàn bộ, mỗi lần kiểm tra nhiều nhất ba vị trí, do đó có nhiều nhất 9 so sánh từ. Phiên bản này không thể trở nên quá chậm dưới những ràng buộc thực tế. 

Tuy nhiên, nếu chúng ta tưởng tượng thao tác tương tự trên (n) từ, việc quét liên tục sau khi xóa có thể yêu cầu so sánh (O(n^2)) trong trường hợp xấu nhất. Cấu trúc của bài toán này cho chúng ta một nhận xét đơn giản hơn: việc loại bỏ một từ không ảnh hưởng đến việc có nên loại bỏ từ nào khác hay không. Mỗi từ có thể được phân loại độc lập bằng cách kiểm tra xem nó có chính xác không`bubble`hoặc chính xác`tapioka`. Điều đó có nghĩa là không cần phải mô phỏng việc loại bỏ nào cả. 

Giải pháp tối ưu làm cho người ta vượt qua ba từ. Nếu một từ có thể tháo rời, chúng tôi bỏ qua nó. Nếu không, chúng tôi sẽ thêm nó vào câu trả lời. Cuối cùng, một câu trả lời trống được thay thế bằng`nothing`. Điều này làm giảm phiên bản tổng quát của vấn đề về thời gian tuyến tính và đơn giản hơn việc sửa đổi đầu vào nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) ở dạng tổng quát, tối đa 9 so sánh ở đây | (O(n)) | Được chấp nhận ở đây | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

Ở đây (n=3) cho bài toán thực tế, vì vậy cả hai cách tiếp cận đều đủ nhanh. Phiên bản một lượt được ưa chuộng hơn vì tính chính xác của nó phụ thuộc trực tiếp vào việc xử lý độc lập từng từ. 

## Hướng dẫn thuật toán 

1. Đọc dòng đầu vào duy nhất và chia nó thành ba từ. Việc phân tách bằng khoảng trắng sẽ cung cấp cho chúng ta các thành phần món ăn riêng lẻ mà không cần phải xử lý các vị trí ký tự theo cách thủ công. 
2. Tạo một danh sách trống cho các từ liên quan đến món ăn thực tế. 
3. Kiểm tra từng từ đầu vào một lần. Nếu chính xác là`bubble`hoặc chính xác`tapioka`, loại bỏ nó. Nếu không, hãy thêm nó vào danh sách kết quả. Sự bình đẳng chính xác là cần thiết bởi vì những từ như`tapiokas`phải sống sót. 
4. Sau khi xử lý xong cả ba từ, hãy kiểm tra xem danh sách kết quả có trống không. Nếu trống thì in`nothing`, bởi vì toàn bộ món ăn bao gồm các từ có thể tháo rời. 
5. Nếu không, hãy nối các từ còn sót lại bằng dấu cách đơn và in chúng. Vì các từ được thêm vào theo thứ tự ban đầu nên thứ tự đầu ra sẽ tự động đúng. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, danh sách kết quả chứa chính xác những từ trong tiền tố đó mà không phải là`bubble`cũng không`tapioka`, theo thứ tự ban đầu của chúng. Khi từ tiếp theo có thể xóa được, việc bỏ qua nó sẽ giữ nguyên thuộc tính này. Khi từ tiếp theo không thể xóa được, việc thêm từ đó cũng giữ nguyên thuộc tính. Vào thời điểm cả ba từ đã được xử lý, kết quả chứa chính xác mọi từ được yêu cầu và không có từ nào có thể xóa được. Nếu kết quả trống thì mọi từ đầu vào đều có thể xóa được, vì vậy`nothing`chính xác là đầu ra cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    words = input().split()

    result = []

    for word in words:
        if word != "bubble" and word != "tapioka":
            result.append(word)

    if result:
        print(" ".join(result))
    else:
        print("nothing")

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng một dòng và chia thành các từ riêng lẻ, khớp với thực tế là tên món ăn chứa chính xác ba từ. các`result`list chỉ lưu trữ những từ còn sót lại sau bước lọc của thuật toán. 

Điều kiện sử dụng tính đẳng thức của chuỗi chính xác thay vì kiểm tra chuỗi con. Đây là phần tinh tế của việc thực hiện:`tapiokas`,`bubbletea`và các từ khác có chứa từ có thể tháo rời vẫn là từ món ăn hợp lệ và không được chạm vào. 

Vòng lặp xử lý mỗi từ chính xác một lần, do đó không có thao tác chỉ mục hoặc thao tác xóa nào có thể gây ra lỗi riêng lẻ. Các chuỗi của Python được so sánh trực tiếp và không có phép tính số nào, do đó việc tràn số nguyên là không liên quan. 

Cuối cùng, kiểm tra`if result`phân biệt hai dạng đầu ra có thể có. Một kết quả không trống được nối bằng dấu cách, trong khi kết quả trống sẽ trở thành`nothing`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`bubble tea pizza`, từ đầu tiên có thể xóa được, trong khi hai từ còn lại vẫn tồn tại. 

| Lời | Có thể tháo rời? | Kết quả sau bước | 
| --- | --- | --- | 
|`bubble`| Có |`[]`| 
|`tea`| Không |`[`trà`]`| 
|`pizza`| Không |`[`trà`, `pizza`]`| 

Danh sách cuối cùng chứa`tea`Và`pizza`, do đó, việc nối nó với khoảng trắng sẽ tạo ra`tea pizza`. Điều này chứng tỏ rằng việc lọc giữ nguyên thứ tự ban đầu của các từ còn sót lại. 

### Mẫu 2 

Đối với đầu vào`tapioka cake tapiokas`, từ đầu tiên sẽ bị loại bỏ. Từ thứ hai và thứ ba tồn tại vì`cake`không liên quan đến khoai mì và`tapiokas`không chính xác`tapioka`. 

| Lời | Có thể tháo rời? | Kết quả sau bước | 
| --- | --- | --- | 
|`tapioka`| Có |`[]`| 
|`cake`| Không |`[`bánh ngọt`]`| 
|`tapiokas`| Không |`[`bánh ngọt`, `khoai mì`]`| 

Đầu ra cuối cùng là`cake tapiokas`. Dấu vết này xác nhận cụ thể rằng việc so sánh phải dành cho từ hoàn chỉnh chứ không phải chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi từ đầu vào được kiểm tra chính xác một lần, với (n=3) ở đây. | 
| Không gian | (O(n)) | Các từ còn sót lại được lưu trữ trong danh sách kết quả. | 

Đối với bài toán thực tế, (n) luôn bằng 3 và mỗi từ có tối đa 32 ký tự, do đó thuật toán chỉ thực hiện một số phép so sánh chuỗi và sử dụng bộ nhớ không đáng kể. Nó thoải mái trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    words = inp.split()

    result = []
    for word in words:
        if word != "bubble" and word != "tapioka":
            result.append(word)

    if result:
        return " ".join(result)
    return "nothing"

# Provided samples
assert solve("bubble tea pizza") == "tea pizza", "sample 1"
assert solve("tapioka cake tapiokas") == "cake tapiokas", "sample 2"
assert solve("tapioka jasmine tea") == "jasmine tea", "sample 3"
assert solve("tapioka bubble tapioka") == "nothing", "sample 4"

# Minimum-size style case: three words with two removable words.
assert solve("bubble tapioka cake") == "cake", "two removable words"

# All three words are removable.
assert solve("tapioka tapioka tapioka") == "nothing", "all removable"

# Exact-word boundary: tapiokas must not be removed.
assert solve("bubble tapiokas bubble") == "tapiokas", "exact word matching"

# Maximum word length and non-removable words.
w = "a" * 32
x = "b" * 32
assert solve(f"bubble {w} {x}") == f"{w} {x}", "maximum word length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`bubble tapioka cake`|`cake`| Nhiều từ có thể tháo rời và một từ còn sót lại | 
|`tapioka tapioka tapioka`|`nothing`| Mọi từ đều bị xóa | 
|`bubble tapiokas bubble`|`tapiokas`| Khớp chính xác thay vì khớp chuỗi con | 
|`bubble`theo sau là hai từ 32 chữ cái | Hai từ dài | Độ dài từ tối đa và bảo toàn các từ thông thường | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là khi mọi từ đều có thể tháo rời. Vì`tapioka bubble tapioka`, vòng lặp bỏ qua cả ba từ, để lại`result`trống. Kiểm tra cuối cùng phát hiện điều này và in`nothing`thay vì in một dòng trống. 

Trường hợp cạnh thứ hai là khớp từ chính xác. Vì`tapioka cake tapiokas`, từ đầu tiên thỏa mãn`word == "tapioka"`và được loại bỏ. từ`tapiokas`không so sánh được và được thêm vào kết quả. Đầu ra cuối cùng là`cake tapiokas`. Một sự thay thế chuỗi con chẳng hạn như thay thế mọi lần xuất hiện của`"tapioka"`bên trong dòng sẽ làm hỏng từ này một cách không chính xác. 

Trường hợp cạnh thứ ba là các từ có thể tháo rời có thể xuất hiện bên cạnh các từ thông thường ở bất kỳ vị trí nào được phép. Vì`bubble tapiokas bubble`, từ thứ nhất và từ thứ ba bị loại bỏ, còn từ ở giữa. Thuật toán tạo ra`tapiokas`, cho thấy rằng nó không phụ thuộc vào việc các từ có thể tháo rời bị giới hạn ở một vị trí cụ thể. 

Trường hợp cạnh thứ tư là độ dài từ tối đa. Với một từ thông thường gồm 32 ký tự, logic so sánh không thay đổi chút nào. Vì Python xử lý trực tiếp các chuỗi hoàn chỉnh nên độ dài từ chỉ ảnh hưởng đến chi phí nhỏ của việc so sánh chuỗi và không tạo ra bất kỳ vấn đề ranh giới nào.
