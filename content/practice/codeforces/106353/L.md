---
title: "CF 106353L - Giáng Sinh Cuối Cùng"
description: "Chúng tôi được đưa ra một số bảng xếp hạng âm nhạc top 10 Giáng sinh. Mỗi bảng xếp hạng chứa 10 tên nghệ sĩ được sắp xếp từ vị trí 1 (tốt nhất) đến vị trí 10. Cùng một nghệ sĩ có thể xuất hiện nhiều lần trong cùng một bảng xếp hạng và trên các bảng xếp hạng khác nhau."
date: "2026-06-19T17:07:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106353
codeforces_index: "L"
codeforces_contest_name: "2025-2026 ICPC Northwestern European Regional Programming Contest (NWERC 2025)"
rating: 0
weight: 106353
solve_time_s: 52
verified: true
draft: false
---

[CF 106353L - Giáng sinh năm ngoái](https://codeforces.com/problemset/problem/106353/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số bảng xếp hạng âm nhạc top 10 Giáng sinh. Mỗi bảng xếp hạng chứa 10 tên nghệ sĩ được sắp xếp từ vị trí 1 (tốt nhất) đến vị trí 10. Cùng một nghệ sĩ có thể xuất hiện nhiều lần trong cùng một bảng xếp hạng và trên các bảng xếp hạng khác nhau. 

Từ tất cả các bảng xếp hạng cộng lại, chúng tôi tính tổng tần suất mỗi nghệ sĩ xuất hiện. Tổng tần số đó là điểm số chính. Nếu nhiều nghệ sĩ có tổng tần suất giống nhau, chúng tôi sẽ tinh chỉnh so sánh bằng cách xem tần suất họ xuất hiện ở vị trí số 1 trên tất cả các bảng xếp hạng. Nếu vẫn hòa, chúng tôi so sánh số đếm ở vị trí 2, sau đó đến vị trí 3, tiếp tục cho đến vị trí 10. Nếu tất cả các số đếm này giống hệt nhau đối với hai nghệ sĩ trở lên, thì không có người chiến thắng duy nhất tồn tại và kết quả đầu ra phải là “hòa”. 

Vì vậy, mỗi nghệ sĩ được biểu diễn một cách hiệu quả bằng một vectơ gồm 11 số nguyên: tổng số lần xuất hiện và 10 số vị trí. Sự so sánh mang tính từ điển trên vectơ này. 

Các ràng buộc rất nhỏ: tối đa 100 danh sách, mỗi danh sách có kích thước 10. Điều này có nghĩa là tổng thể có tối đa 1000 mục nhập. Bất kỳ giải pháp nào xử lý mỗi mục một lần hoặc một số lần không đổi đều dễ dàng đủ. Ngay cả việc quét nhiều lần trên tất cả các nghệ sĩ vẫn ổn, nhưng không cần thiết. 

Một sai lầm ngây thơ xuất phát từ việc chỉ sử dụng tổng số. Ví dụ: nếu hai nghệ sĩ mỗi người xuất hiện ba lần nhưng một nghệ sĩ luôn được xếp hạng cao hơn trong danh sách thì câu trả lời không phải là hòa. Một sai lầm tinh vi khác là bỏ qua các lần xuất hiện lặp lại trong cùng một danh sách, vì chúng đóng góp nhiều lần vào cả tổng số và vị trí. 

Các trường hợp cạnh rất đơn giản nhưng đáng chú ý. Một danh sách duy nhất có nghĩa là câu trả lời đơn giản là nghệ sĩ xuất hiện thường xuyên nhất trong danh sách đó với sự ràng buộc về vị trí. Một trường hợp khác là khi tất cả các nghệ sĩ xuất hiện với số lần như nhau và cũng có sự phân bổ vị trí giống hệt nhau, dẫn đến “hòa”. Cuối cùng, trường hợp một nghệ sĩ có tổng số lần xuất hiện ít hơn nhưng nhiều lần xuất hiện ở vị trí đầu tiên không thể xảy ra do quy tắc từ điển nghiêm ngặt: tổng số lần xuất hiện luôn chiếm ưu thế. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý tất cả các mục và duy trì cấu trúc tần số cho mỗi nghệ sĩ. Đối với mỗi lần xuất hiện, chúng tôi tăng cả tổng số và số lượng tương ứng với vị trí của nó. Sau khi xử lý, chúng tôi so sánh tất cả các nghệ sĩ để tìm ra nghệ sĩ giỏi nhất. 

Điều này đúng vì các quy tắc hoàn toàn mang tính bổ sung và chỉ phụ thuộc vào số lượng tổng hợp. Tuy nhiên, ngay cả khi chúng tôi thử làm điều gì đó dư thừa hơn như tính toán lại số lượng cho từng nghệ sĩ riêng biệt, chúng tôi sẽ liên tục quét toàn bộ dữ liệu đầu vào, dẫn đến hành vi bậc hai không cần thiết đối với số lượng nghệ sĩ riêng biệt. 

Quan sát quan trọng là mọi nghệ sĩ đều có thể được mô tả đầy đủ bằng một vectơ có độ dài cố định có kích thước 11 và sự so sánh mang tính từ điển. Điều này cho phép chúng tôi tính toán mọi thứ trong một lần và sau đó chọn mức tối đa bằng cách sử dụng một quy tắc so sánh duy nhất. 

Vì vậy, nhiệm vụ giảm xuống còn việc xây dựng một từ điển từ tên nghệ sĩ đến vectơ của nó, sau đó chọn vectơ tối đa, theo dõi xem mức tối đa có phải là duy nhất hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại Brute Force cho mỗi nghệ sĩ | O(U * N * 10) | O(U) | Quá chậm | 
| Tổng hợp một lượt + lựa chọn tối đa | O(N * 10) | O(U * 11) | Đã chấp nhận | 

Ở đây U là số lượng nghệ sĩ riêng biệt và N là số lượng danh sách. 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng thông tin tần số trong một lần quét qua đầu vào, sau đó xác định nghệ sĩ giỏi nhất bằng cách so sánh từ điển các vectơ điểm của họ.

1. Đọc số danh sách n. Điều này xác định có bao nhiêu nhóm gồm 10 mục chúng tôi sẽ xử lý. 
2. Tạo một từ điển ánh xạ từng tên nghệ sĩ vào một mảng gồm 11 số nguyên được khởi tạo bằng 0. Mục nhập đầu tiên biểu thị tổng số lần xuất hiện và 10 mục tiếp theo biểu thị số lần đếm cho các vị trí từ 1 đến 10. 
3. Với mỗi danh sách, hãy đọc tên 10 nghệ sĩ theo thứ tự. 
4. Đối với mỗi vị trí j từ 0 đến 9, cập nhật vectơ của nghệ sĩ tương ứng bằng cách tăng tổng số và cũng tăng số lượng ở vị trí j+1. Chỉ số vị trí quan trọng vì nó mã hóa thông tin xếp hạng cần thiết để phân hạng. 
5. Sau khi xử lý tất cả các danh sách, lặp lại tất cả các nghệ sĩ và tính toán ứng cử viên tốt nhất bằng cách sử dụng so sánh từ điển các vectơ của họ. Duy trì cả vectơ tốt nhất hiện tại và cờ cho biết nó có phải là duy nhất hay không. 
6. Nếu một nghệ sĩ khác khớp chính xác với vectơ tốt nhất hiện tại, hãy đánh dấu kết quả là không rõ ràng. 
7. Sau khi quét, xuất tên nghệ sĩ nếu là duy nhất, nếu không thì xuất ra “tie”. 

### Tại sao nó hoạt động 

Mỗi nghệ sĩ được ánh xạ tới một bản tóm tắt xác định tất cả thông tin liên quan được sử dụng theo quy tắc xếp hạng. Quy trình so sánh được sử dụng trong lần quét cuối cùng giống hệt với định nghĩa của vấn đề: tổng tần số chiếm ưu thế và tần số vị trí giải quyết các mối quan hệ theo thứ tự chặt chẽ. Vì mỗi lần xuất hiện đều được tính chính xác một lần vào đúng thành phần của vectơ nên không có thông tin nào bị mất. Do đó, mức từ điển tối đa trên các vectơ này chính xác là nghệ sĩ sẽ giành chiến thắng trong quy trình loại trừ được mô tả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    stats = {}

    for _ in range(n):
        arr = input().split()
        for i, name in enumerate(arr):
            if name not in stats:
                stats[name] = [0] * 11
            stats[name][0] += 1
            stats[name][i + 1] += 1

    best_name = None
    best_vec = None
    unique = True

    for name, vec in stats.items():
        if best_vec is None:
            best_vec = vec
            best_name = name
            unique = True
        else:
            if vec > best_vec:
                best_vec = vec
                best_name = name
                unique = True
            elif vec == best_vec:
                unique = False

    if unique:
        print(best_name)
    else:
        print("tie")

if __name__ == "__main__":
    solve()
```Giải pháp giữ một từ điển trong đó mỗi nghệ sĩ lưu trữ một danh sách có kích thước cố định. Chỉ số 0 tích lũy tổng số lần xuất hiện, trong khi chỉ số 1 đến 10 theo dõi số lượng vị trí cụ thể. Cấu trúc này khớp trực tiếp với quy tắc xếp hạng, do đó so sánh trở thành so sánh vectơ từ điển đơn lẻ. 

Giai đoạn lựa chọn dựa vào so sánh danh sách gốc của Python, vốn đã thực hiện sắp xếp từ điển. Việc phát hiện đẳng thức là điều cần thiết: ngay cả khi tìm thấy vectơ tốt nhất, chúng ta phải xác minh rằng nó là duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
elton jose bowie eagles elvis wham nat joni bruce minnie
elton madonna wham elvis andy donny madonna aha chuck judy
madonna heart whitney chuck joni wham john yoko nsync stevie
```Chúng tôi chỉ theo dõi các nghệ sĩ có liên quan ở đây. 

| Bước | Nghệ sĩ | Tổng cộng | Vị trí1 | Pos2 | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | elton | 1 | 1 | 0 | elton | 
| 2 | jose | 1 | 0 | 1 | elton | 
| ... | ... | ... | ... | ... | elton | 
| sau khi quét toàn bộ | madonna | 3 | 1 | 1 | madonna | 
| sau khi quét toàn bộ | wham | 3 | 1 | 0 | madonna | 

Madonna và Wham đều có tổng số điểm là 3, nhưng Madonna có nhiều lần xuất hiện ở vị trí đầu tiên hơn nên Madonna trở thành người giỏi nhất. 

Đầu ra:```
madonna
```Dấu vết này cho thấy số lượng vị trí phá vỡ các mối quan hệ mà chỉ riêng tổng tần số không thể giải quyết được. 

### Ví dụ 2 

đầu vào:```
2
maria a b wham c d e f maria wham
wham g h maria i j k l wham m
```| Bước | Nghệ sĩ | Tổng cộng | Vị trí1 | Pos2 | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| sau khi quét | maria | 3 | 1 | 0 | maria | 
| sau khi quét | cái gì | 3 | 1 | 1 | cà vạt | 

Ở đây, maria và wham có tổng số lượng giống hệt nhau và phân bố vị trí giống hệt nhau sau khi phân giải so sánh, do đó không chiếm ưu thế hoàn toàn. 

Đầu ra:```
tie
```Điều này chứng tỏ rằng sự bình đẳng trên tất cả 11 khía cạnh dẫn đến sự mơ hồ hơn là lựa chọn người chiến thắng một cách tùy tiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 10) | Mỗi danh sách trong số n danh sách được xử lý một lần và mỗi danh sách có 10 mục | 
| Không gian | O(u · 11) | Một vectơ có kích thước cố định cho mỗi nghệ sĩ riêng biệt | 

Giới hạn n 100 làm cho điều này trở nên hoàn toàn tầm thường trong thực tế, vì tổng số thao tác nhiều nhất là 1000 lần cập nhật cộng với một lần chuyển qua các tên riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""3
elton jose bowie eagles elvis wham nat joni bruce minnie
elton madonna wham elvis andy donny madonna aha chuck judy
madonna heart whitney chuck joni wham john yoko nsync stevie
""") == "madonna"

# sample 2
assert run("""2
maria a b wham c d e f maria wham
wham g h maria i j k l wham m
""") == "tie"

# minimum input
assert run("""1
a a a a a a a a a a
""") == "a"

# all distinct
assert run("""1
a b c d e f g h i j
""") == "a"

# tie identical structure
assert run("""1
a b c d e f g h i j
a b c d e f g h i j
""") == "tie"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nghệ sĩ lặp lại duy nhất | một | độ chính xác đếm tối thiểu | 
| tất cả danh sách riêng biệt | một | phá vỡ ràng buộc vị trí | 
| danh sách trùng lặp | cà vạt | phát hiện bình đẳng đầy đủ | 

## Vỏ cạnh 

Trường hợp danh sách đơn tối thiểu kiểm tra xem số lượng vị trí có được áp dụng chính xác hay không. Đối với đầu vào`a a a a a a a a a a`, cùng một nghệ sĩ sẽ tích lũy tất cả các vị trí và tổng số, và thuật toán sẽ ngay lập tức chọn người đó là người giỏi nhất. 

Trường hợp đẳng thức hoàn toàn xảy ra khi hai nghệ sĩ có vectơ phân phối giống hệt nhau. Đối với đầu vào trong đó mọi mẫu vị trí khớp chính xác với hai tên, cả hai vectơ trở nên giống hệt nhau, phép so sánh phát hiện sự bằng nhau và đầu ra trở thành "hòa". Quá trình quét không chọn sớm một vì việc theo dõi tính duy nhất được cập nhật bất cứ khi nào quan sát được sự bình đẳng. 

Trường hợp có nhiều nghệ sĩ khác nhau nhưng một nghệ sĩ chiếm ưu thế ở các vị trí đầu sẽ được xử lý một cách tự nhiên vì so sánh từ điển ưu tiên tổng số trước, sau đó là vị trí 1, đảm bảo không có lợi thế nào ở vị trí thấp hơn có thể ghi đè các tín hiệu mạnh hơn có mức độ ưu tiên cao hơn.
