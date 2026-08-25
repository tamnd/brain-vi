---
title: "CF 104312D - Tình yêu là chiến tranh"
description: "Chúng ta được cung cấp một tập hợp các tin nhắn văn bản ngắn, mỗi tin nhắn này độc lập với những tin nhắn khác. Đối với mỗi tin nhắn, chúng ta cần quyết định xem nó đại diện cho một “trận chiến” hay chỉ là một cuộc trò chuyện thông thường."
date: "2026-07-01T19:52:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104312
codeforces_index: "D"
codeforces_contest_name: "UTPC Spring 2023 Contest (HS)"
rating: 0
weight: 104312
solve_time_s: 65
verified: true
draft: false
---

[CF 104312D - Tình yêu là chiến tranh](https://codeforces.com/problemset/problem/104312/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các tin nhắn văn bản ngắn, mỗi tin nhắn này độc lập với những tin nhắn khác. Đối với mỗi tin nhắn, chúng ta cần quyết định xem nó đại diện cho một “trận chiến” hay chỉ là một cuộc trò chuyện thông thường. 

Một tin nhắn được coi là một trận chiến nếu nó chứa một chuỗi liền kề gồm ít nhất ba ký tự đều là chữ cái 'a' và không phân biệt chữ hoa chữ thường. Điều này có nghĩa là chúng tôi coi phiên bản chữ hoa và chữ thường của các chữ cái là giống hệt nhau, do đó, các chuỗi như “aaa”, “Aaa” hoặc “aAAa” đều đủ điều kiện miễn là có một dãy độ dài ít nhất ba chữ cái giống 'a' liên tiếp trong văn bản. 

Mỗi dòng đầu vào được kiểm tra riêng và chúng tôi xuất ra một cụm từ cố định tùy thuộc vào việc chuỗi con đó có tồn tại hay không. 

Các ràng buộc rất nhỏ: mỗi tin nhắn có độ dài tối đa 100 ký tự và ngay cả khi số lượng tin nhắn lớn thì tổng công việc trên mỗi tin nhắn vẫn bị giới hạn không đổi. Điều này ngay lập tức loại trừ mọi thứ nặng hơn quét tuyến tính trên mỗi chuỗi và thậm chí hành vi bậc hai trên mỗi chuỗi cũng có thể chấp nhận được nhưng không cần thiết. Một lần cho mỗi tin nhắn là đủ. 

Sự tinh tế chính là xác định chính xác các chữ “a liên tiếp” trong trường hợp không phân biệt chữ hoa chữ thường và đảm bảo chúng tôi đặt lại số đếm chính xác bất cứ khi nào ký tự không phải ‘a’ xuất hiện. 

Một lỗi phổ biến xuất phát từ việc chỉ kiểm tra chữ thường chính xác 'a' mà không chuẩn hóa. Ví dụ: “AAA” phải là một trận chiến, nhưng việc kiểm tra phân biệt chữ hoa chữ thường sẽ từ chối nó một cách không chính xác. 

Một trường hợp khó phát hiện khác là quên đặt lại dấu cách hoặc dấu câu. Ví dụ: “aa a aaaa” chứa một chuỗi hợp lệ gồm bốn chữ a liên tiếp ở cuối, nhưng nếu một lập trình viên đặt lại nhầm ở mọi khoảng trống hoặc hợp nhất các phân đoạn không liền kề, họ có thể tính sai. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi vị trí trong chuỗi, hãy thử mở rộng một cửa sổ và kiểm tra xem liệu chúng ta có thể tìm thấy một chuỗi gồm ít nhất ba ký tự liên tiếp đều là 'a' sau khi chuẩn hóa kiểu chữ hay không. Đối với mỗi chỉ mục bắt đầu, việc quét tiến lên để phát hiện một lần chạy mất O(n) và thực hiện việc này cho mọi chỉ mục sẽ mang lại O(n²) cho mỗi tin nhắn. Với tối đa 100 ký tự, điều này vẫn hoạt động thoải mái, nhưng nó là quá mức cần thiết. 

Điều quan trọng là chúng ta không cần phải khởi động lại quá trình quét từ mọi vị trí. Chúng tôi chỉ quan tâm đến các lần chạy liền kề của cùng một loại nhân vật. Nếu chúng tôi duy trì số lượng ký tự loại 'a' đang chạy liên tiếp trong khi quét từ trái sang phải, chúng tôi có thể quyết định ngay trong một lần xem liệu bất kỳ lần chạy nào có đạt đến độ dài ba hay không. Điều này biến vấn đề thành một kiểm tra phát trực tuyến đơn giản: tích lũy các vệt, đặt lại khi bị hỏng và theo dõi độ dài vệt tối đa. 

Cấu trúc của bài toán về cơ bản dựa trên độ dài chạy, do đó việc thu gọn chuỗi thành các đoạn liên tiếp là tối ưu. Mỗi ký tự được xử lý chính xác một lần và trạng thái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi tin nhắn | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) mỗi tin nhắn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển toàn bộ thông báo sang chữ thường để sự khác biệt về chữ hoa không ảnh hưởng đến việc so sánh. Điều này đảm bảo xử lý thống nhất 'A' và 'a' mà không cần thêm logic có điều kiện trong quá trình quét. 
2. Khởi tạo bộ đếm`streak = 0`để theo dõi số lượng ký tự 'a' liên tiếp hiện tại. 
3. Quét chuỗi từ trái sang phải, xử lý từng ký tự một. Mỗi nhân vật góp phần vào việc tiếp tục một chuỗi hoặc phá vỡ nó. 
4. Nếu ký tự hiện tại là ‘a’, hãy tăng dần`streak`bởi một. Điều này mở rộng phân đoạn liền kề hiện tại của các ký tự hợp lệ. 
5. Nếu ký tự hiện tại không phải là ‘a’, hãy đặt lại`streak`về 0 vì yêu cầu liên tục bị phá vỡ. 
6. Sau khi cập nhật`streak`, kiểm tra xem nó đã đạt tới 3 hay nhiều hơn. Nếu có, chúng tôi có thể ngay lập tức phân loại tin nhắn là một trận chiến và ngừng quét sớm. 
7. Nếu chúng tôi quét xong mà không đạt đến chuỗi 3, hãy phân loại thư không phải là một trận chiến. 

Việc dừng sớm là tùy chọn nhưng phù hợp với thực tế là khi tìm thấy phân đoạn hợp lệ, việc xử lý tiếp theo không thể thay đổi câu trả lời. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào trong chuỗi,`streak`biểu thị chính xác độ dài của hậu tố hiện tại của các ký tự 'a' liên tiếp kết thúc tại vị trí đó. Bởi vì chúng tôi đặt lại nó ngay lập tức khi ký tự không phải 'a' xuất hiện nên nó luôn mã hóa một phân đoạn liền kề hợp lệ kết thúc ở chỉ mục hiện tại. Bất kỳ chuỗi con hợp lệ nào có độ dài ít nhất là 3 đều phải kết thúc ở một chỉ mục nào đó trong đó`streak >= 3`, vì vậy việc phát hiện trạng thái như vậy là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        s = input().rstrip("\n").lower()
        
        streak = 0
        found = False
        
        for ch in s:
            if ch == 'a':
                streak += 1
                if streak >= 3:
                    found = True
                    break
            else:
                streak = 0
        
        if found:
            print("Love is war!")
        else:
            print("Just friends <3")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng tin nhắn, chuẩn hóa nó bằng cách sử dụng`.lower()`và duy trì một bộ đếm số nguyên duy nhất. các`found`cờ cho phép chấm dứt sớm sau khi phát hiện một lần chạy hợp lệ. Đặt lại`streak`trên bất kỳ ký tự không phải 'a' nào đều duy trì tính liền kề, đó là yêu cầu cốt lõi. 

Một chi tiết triển khai tinh tế là chỉ loại bỏ ký tự dòng mới. Chúng tôi tránh loại bỏ khoảng trắng vì khoảng trắng là những ký tự có ý nghĩa ngắt dòng. Một điểm quan trọng khác là thực hiện`.lower()`một lần trên mỗi chuỗi thay vì trên mỗi ký tự, điều này giữ cho vòng lặp trên mỗi ký tự ở mức tối thiểu và hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
THIS MEANS WAAAAAARRRRRRRRRR
```Chúng tôi theo dõi quá trình quét: 

| Chỉ mục | Char | Hạ | Vệt | Đã tìm thấy | 
| --- | --- | --- | --- | --- | 
| 0 | T | t | 0 | không | 
| ... | ... | ... | 0 | không | 
| 11 | W | w | 0 | không | 
| 12 | A | một | 1 | không | 
| 13 | A | một | 2 | không | 
| 14 | A | một | 3 | vâng | 

Ở chỉ số 14, chuỗi đạt tới 3 nên chúng tôi ngay lập tức xếp nó vào loại trận chiến. Điều này chứng tỏ rằng chỉ có các lần chạy liền kề mới quan trọng chứ không phải tổng số. 

Bây giờ hãy xem xét:```
LlLmmAAaOoo that's soooo funnnyyy
```| Chỉ mục | Char | Hạ | Vệt | Đã tìm thấy | 
| --- | --- | --- | --- | --- | 
| ... | tôi | tôi | 0 | không | 
| ... | m | m | 0 | không | 
| ... | A | một | 1 | không | 
| ... | A | một | 2 | không | 
| ... | một | một | 3 | vâng | 

Mặc dù sau đó các chữ cái được phân tách bằng các ký tự khác nhưng dòng chữ “AAA” vẫn xuất hiện cục bộ và gây ra tình trạng này. 

Điều này xác nhận rằng thuật toán tách biệt chính xác các phân đoạn liền kề và bỏ qua các phần không liên quan của chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số ký tự trên tất cả tin nhắn) | Mỗi ký tự được truy cập một lần và được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số bộ đếm và cờ được sử dụng | 

Vì mỗi tin nhắn có tối đa 100 ký tự nên tổng khối lượng công việc cực kỳ nhỏ, nằm trong giới hạn ngay cả đối với n lớn. Thuật toán có kích thước đầu vào tuyến tính một cách hiệu quả với chi phí không đổi tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    n = int(sys.stdin.readline())
    for _ in range(n):
        s = sys.stdin.readline().rstrip("\n").lower()
        streak = 0
        found = False
        for ch in s:
            if ch == 'a':
                streak += 1
                if streak >= 3:
                    found = True
                    break
            else:
                streak = 0
        
        output.append("Love is war!" if found else "Just friends <3")
    
    return "\n".join(output)

# provided samples
assert run("""5
How are you doing Kaguya?
THIS MEANS WAAAAAARRRRRRRRRR
oops sorry wrong person
LlLmmAAaOoo that's soooo funnnyyy
what is a aardvark
""") == """Just friends <3
Love is war!
Just friends <3
Love is war!
Just friends <3"""

# custom cases
assert run("1\naaa") == "Love is war!"
assert run("1\nAaA") == "Love is war!"
assert run("1\naa a aa") == "Just friends <3"
assert run("1\nbbbbbaaa") == "Love is war!"
assert run("1\nababa") == "Just friends <3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaa`| Tình yêu là chiến tranh! | vệt hợp lệ tối thiểu | 
|`AaA`| Tình yêu là chiến tranh! | trường hợp không nhạy cảm | 
|`aa a aa`| Chỉ là bạn bè <3 | thiết lập lại không liền kề | 
|`bbbbbaaa`| Tình yêu là chiến tranh! | phát hiện ở cuối | 
|`ababa`| Chỉ là bạn bè <3 | không có phân đoạn hợp lệ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi chuỗi chứa các chữ cái viết hoa hỗn hợp tạo thành một lần chạy hợp lệ chỉ sau khi chuẩn hóa. Ví dụ: đầu vào “AaAaA” trở thành “aaaaa” sau khi hạ thấp, tạo ra một vệt hợp lệ là 5. Thuật toán xử lý điều này vì quá trình chuẩn hóa diễn ra trước khi quét, do đó số vệt được tính toán trên một biểu diễn thống nhất. 

Một trường hợp khác là khi các ký tự hợp lệ được phân tách bằng dấu cách hoặc dấu chấm câu. Ví dụ: “aa aaaa” phải là một trận chiến vì có bốn chữ ‘a’ kéo dài. Trong quá trình quét, khoảng trắng sẽ đặt lại vệt về 0 và phân đoạn cuối cùng sẽ xây dựng lại vệt một cách chính xác, đạt đến 4 và kích hoạt tình trạng. 

Trường hợp thứ ba là các chuỗi hoàn toàn không có 'a', chẳng hạn như "hello world". Kỉ lục này vẫn bằng 0 xuyên suốt và không có sự kết thúc sớm nào xảy ra, tạo ra sự phân loại chính xác không phải trận chiến.
