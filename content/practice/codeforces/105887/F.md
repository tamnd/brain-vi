---
title: "CF 105887F - \u8d62\u9ebb\u4e86"
description: "Mỗi bài kiểm tra mô tả một trò chơi so sánh đơn giản giữa ba giá trị: sức mạnh của Alice a, sức mạnh của Bob b và sức mạnh của Candy c. Đầu ra là một chuỗi được chọn bằng cách áp dụng quy tắc ưu tiên đối với các so sánh này. Quá trình quyết định là tuần tự."
date: "2026-06-21T15:06:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105887
codeforces_index: "F"
codeforces_contest_name: "\u7b2c\u5341\u4e09\u5c4a\u91cd\u5e86\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 105887
solve_time_s: 50
verified: true
draft: false
---

[CF 105887F - \u8d62\u9ebb\u4e86](https://codeforces.com/problemset/problem/105887/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi bài kiểm tra mô tả một trò chơi so sánh đơn giản giữa ba giá trị: Sức mạnh của Alice`a`, sức mạnh của Bob`b`và sức mạnh của Candy`c`. Đầu ra là một chuỗi được chọn bằng cách áp dụng quy tắc ưu tiên đối với các so sánh này. 

Quá trình quyết định là tuần tự. Đầu tiên, Alice cố gắng xác định liệu cô ấy có trực tiếp đánh bại Bob hay không, tương ứng với việc kiểm tra xem liệu cô ấy có trực tiếp đánh bại Bob hay không.`a > b`. Nếu điều này đúng, câu trả lời được sửa là`"Win"`và không cần lý luận gì thêm. 

Nếu Alice không trực tiếp đánh bại Bob, trò chơi sẽ chuyển sang cách hiểu gián tiếp: Bob bị coi là yếu hơn nếu Candy mạnh hơn Bob, nghĩa là`c > b`. Trong trường hợp đó, đầu ra trở thành`"WIN"`. 

Nếu cả hai điều kiện đều không thỏa mãn, nghĩa là Alice không đánh bại Bob và Candy cũng không đánh bại Bob, kết quả là`"nowin"`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm chứa ba số nguyên và mỗi số nguyên phải được đánh giá theo cùng một logic. Các ràng buộc đủ nhỏ để mỗi bài kiểm tra có thể được xử lý trong thời gian không đổi. Ngay cả ở mức tối đa 100 trường hợp thử nghiệm, bất kỳ giải pháp O(T) hoặc O(T log T) nào cũng đủ tầm thường và không cần tiền xử lý hoặc cấu trúc dữ liệu. 

Sự tinh tế chính là thứ tự nghiêm ngặt của các điều kiện. Một sai lầm phổ biến là kiểm tra`c > b`trước`a > b`, điều này sẽ phân loại không chính xác các trường hợp Alice đã thắng trực tiếp. Một trường hợp khác là khi các giá trị bằng nhau, đặc biệt`a == b`hoặc`c == b`, cả hai đều phải được coi là thất bại của các bất đẳng thức chặt chẽ tương ứng của chúng. 

Ví dụ, khi`a = 6, b = 6, c = 100`, Alice không đánh bại Bob, nhưng Candy thì đánh bại, nên kết quả đúng là`"WIN"`. Nếu như`a = 10, b = 5, c = 0`, đầu ra đúng là`"Win"`mặc dù Candy không liên quan. Nếu như`a = 5, b = 10, c = 7`, không có điều kiện nào đúng, vì vậy đầu ra là`"nowin"`. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô hình hóa câu chuyện theo nghĩa đen: mô phỏng sự so sánh, thậm chí có thể lý luận về những “chiến thắng” chuyển tiếp giữa các nhân vật. Người ta có thể cố gắng xây dựng một biểu đồ mối quan hệ trong đó một nút được coi là chiến thắng nếu nó đánh bại nút khác hoặc gián tiếp mạnh hơn thông qua Candy. Một mô hình như vậy là không cần thiết vì các quy tắc chỉ chia thành hai so sánh trực tiếp. Ngay cả khi được triển khai, mô phỏng này vẫn sẽ đánh giá số cạnh không đổi cho mỗi lần kiểm tra, nhưng nó gây ra sự phức tạp mà không mang lại lợi ích gì. 

Sự đơn giản hóa chính là thừa nhận rằng toàn bộ trò chơi được xác định bằng cây quyết định có mức độ ưu tiên cố định. Điều kiện đầu tiên`a > b`xác định đầy đủ kết quả khi đúng, điều đó có nghĩa là mọi lý do tiếp theo đều không liên quan trong nhánh đó. Chỉ khi thất bại chúng ta mới kiểm tra điều kiện thứ hai`c > b`. Nếu cả hai đều thất bại, chúng tôi mặc định ở trạng thái cuối cùng. 

Điều này làm giảm vấn đề từ bất kỳ hình thức lý luận quan hệ nào sang việc kiểm tra có điều kiện theo thứ tự nghiêm ngặt. Cấu trúc về cơ bản là một hàm quyết định hai cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng quan hệ ngây thơ | O(T) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Đánh giá có điều kiện trực tiếp | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`T`. Mỗi trường hợp thử nghiệm là độc lập, do đó không có trạng thái nào được thực hiện qua các lần lặp. 
2. Với mỗi test, hãy đọc ba số nguyên`a`,`b`, Và`c`. Chúng đại diện cho sức mạnh cố định và không có sự biến đổi nào được áp dụng cho chúng. 
3. Kiểm tra xem`a > b`. Nếu điều kiện này được giữ thì xuất ngay`"Win"`. Bước này đến trước vì vấn đề xác định nó là quy tắc có mức độ ưu tiên cao nhất. 
4. Nếu`a > b`là sai, hãy kiểm tra xem`c > b`. Nếu điều kiện này được giữ thì xuất ra`"WIN"`. Điều này thể hiện cách giải thích thứ hai trong đó Candy xác định sự thua cuộc của Bob. 
5. Nếu cả hai điều kiện đều không được thỏa mãn, xuất ra`"nowin"`. Điều này tương ứng với khu vực còn lại mà Bob không bị Alice hoặc Candy đánh bại. 

### Tại sao nó hoạt động 

Logic phân vùng toàn bộ không gian của`(a, b, c)`thành ba vùng riêng biệt được xác định bởi sự bất bình đẳng nghiêm ngặt. Điều kiện đầu tiên nắm bắt đầy đủ tất cả các trường hợp trong đó Alice chiếm ưu thế hơn Bob và nó được kiểm tra trước để không có quy tắc thay thế nào ghi đè lên nó. Điều kiện thứ hai chỉ kích hoạt khi Alice thất bại và nó chỉ dựa vào sự so sánh của Candy với Bob. Vì cả hai hoạt động kiểm tra đều loại trừ lẫn nhau theo thứ tự thực hiện và toàn diện khi kết hợp với dự phòng cuối cùng, nên mọi đầu vào đều ánh xạ tới chính xác một đầu ra mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        if a > b:
            print("Win")
        elif c > b:
            print("WIN")
        else:
            print("nowin")

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp kiểm thử một lần và xử lý từng trường hợp một cách độc lập. Cấu trúc phân nhánh phản ánh trực tiếp cây quyết định, đảm bảo mức độ ưu tiên của các điều kiện được bảo toàn chính xác như đã chỉ định. 

Chi tiết triển khai quan trọng là việc sử dụng`elif`. Điều này đảm bảo rằng một khi`a > b`được thỏa mãn, chương trình không đánh giá nhầm điều kiện thứ hai. Nếu không có cấu trúc này, một sự riêng biệt`if`vì`c > b`có thể ghi đè sai kết quả đầu ra chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1 

đầu vào:```
a = 6, b = 500, c = 1000
```| Bước | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 | a > b | Sai | 
| 2 | c > b | Đúng | 
| 3 | Đầu ra | THẮNG | 

Điều này thể hiện quy tắc thứ hai chỉ được kích hoạt sau khi quy tắc đầu tiên thất bại. 

### Ví dụ 2 

đầu vào:```
a = 6, b = 5, c = 1000
```| Bước | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 | a > b | Đúng | 
| 2 | (bỏ qua) | - | 
| 3 | Đầu ra | Thắng | 

Điều này cho thấy giá trị của Candy không còn phù hợp khi Alice trực tiếp đánh bại Bob. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu số lượng so sánh không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép tối đa 100 trường hợp kiểm thử, do đó, việc quét tuyến tính với công việc liên tục cho mỗi trường hợp là hoàn toàn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample (interpreted with T = 5)
assert run("""5
6 5 1000
6 500 1000
6 5000 1000
1234 4009 4039
6 6 6
""") == """Win
WIN
nowin
WIN
nowin"""

# all equal
assert run("""1
10 10 10
""") == "nowin"

# Alice wins directly
assert run("""1
7 3 100
""") == "Win"

# Candy determines win
assert run("""1
3 10 20
""") == "WIN"

# boundary equality case
assert run("""1
5 5 6
""") == "WIN"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | bây giờ | bất đẳng thức chặt chẽ | 
| a > b chiếm ưu thế | Thắng | ưu tiên của quy tắc đầu tiên | 
| c > b chỉ | THẮNG | kích hoạt quy tắc thứ hai | 
| cạnh b bằng nhau | THẮNG hay ngay bây giờ tùy | xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các giá trị đều bằng nhau. Đối với đầu vào`a = b = c = 10`, thuật toán kiểm tra`a > b`thất bại, sau đó kiểm tra`c > b`điều đó cũng thất bại, tạo ra`"nowin"`. Điều này phù hợp với cấu trúc quy tắc vì cả Alice và Candy đều không vượt quá Bob. 

Một trường hợp khác là khi chỉ có sự bình đẳng mới phân biệt được kết quả, chẳng hạn như`a = 5, b = 5, c = 6`. Ở đây, điều kiện đầu tiên không thành công do sự bằng nhau, nhưng điều kiện thứ hai thành công vì Candy vượt quá Bob, tạo ra`"WIN"`. Điều này xác nhận rằng sự bình đẳng không kích hoạt bất kỳ điều kiện thắng nào. 

Trường hợp cuối cùng là khi Alice mạnh hơn hoàn toàn bất kể Candy, chẳng hạn như`a = 100, b = 1, c = 0`. Thuật toán dừng ngay ở lần kiểm tra đầu tiên và trả về`"Win"`, chứng tỏ Candy chưa bao giờ được đánh giá ở nhánh này.
