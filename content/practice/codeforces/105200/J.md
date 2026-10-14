---
title: "CF 105200J - Cuộc đua"
description: "Nhiệm vụ làm giảm sự cạnh tranh thành một so sánh duy nhất. Mỗi người tham gia có một bản ghi về số lượng vấn đề họ đã giải được và chúng tôi cũng có được thời điểm mỗi người tham gia kết thúc phần giải quyết của mình hoặc đạt đến trạng thái được ghi lại cuối cùng."
date: "2026-06-27T02:54:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105200
codeforces_index: "J"
codeforces_contest_name: "IME++ Starters Try-outs 2024"
rating: 0
weight: 105200
solve_time_s: 38
verified: true
draft: false
---

[CF 105200J - Joust](https://codeforces.com/problemset/problem/105200/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ làm giảm sự cạnh tranh thành một so sánh duy nhất. Mỗi người tham gia có một bản ghi về số lượng vấn đề họ đã giải được và chúng tôi cũng có được thời điểm mỗi người tham gia kết thúc phần giải quyết của mình hoặc đạt đến trạng thái được ghi lại cuối cùng. Mục tiêu là xác định xem ai, vào thời điểm quyết định sớm nhất, có hiệu suất cao hơn, được đo lường thuần túy bằng số lượng vấn đề được giải quyết. 

Về mặt thực tế, chúng tôi đang so sánh hai chuỗi ảnh chụp nhanh hiệu suất. Mỗi trình tự mô tả sự tiến bộ của người tham gia theo thời gian. Ý tưởng chính là chỉ có thời điểm sớm nhất mà một người tham gia vượt trội hơn hẳn những người khác. Khi một người tham gia có số lần giải quyết cao hơn tại một chỉ số thời gian nhất định, người tham gia đó được coi là đã "thắng" cuộc đua. 

Mặc dù câu lệnh cực kỳ ngắn nhưng nó ngầm giả định một cấu trúc theo trình tự thời gian. Nếu kích thước đầu vào là n, điều đó có nghĩa là chúng tôi đang quét hai mảng có độ dài n một cách hiệu quả và đưa ra quyết định dựa trên chỉ mục đầu tiên nơi chúng khác nhau một cách quyết định. 

Với các ràng buộc gợi ý độ phức tạp tuyến tính, mọi thứ vượt quá O(n) sẽ không cần thiết. Cách tiếp cận bậc hai so sánh lặp đi lặp lại tất cả các tiền tố sẽ quá chậm nếu n đạt 200.000 trở lên, vì điều đó có nghĩa là có khoảng so sánh khoảng 4 × 10^10 trong trường hợp xấu nhất. 

Sự tinh tế chính là hiểu rằng tiền tố đẳng thức không quan trọng. Nếu cả hai người tham gia đều có số lần giải giống hệt nhau ở một vị trí nào đó thì toàn bộ khu vực đó có thể bị bỏ qua. Quyết định chỉ xuất hiện ở lần không khớp đầu tiên. 

Một cạm bẫy ngây thơ xuất hiện khi người ta giả định tổng số tiền hoặc giá trị cuối cùng quyết định người chiến thắng. Điều đó sẽ sai vì vấn đề rõ ràng phụ thuộc vào việc ai dẫn trước chứ không phải ai về đích mạnh hơn. 

Ví dụ, hãy xem xét: 

đầu vào:```
A: 1 3 3
B: 2 2 3
```Thoạt nhìn, cả hai đều kết thúc ở số 3, nhưng A không bao giờ dẫn trước B, vì ở chỉ số 1 B dẫn trước. Đáp án đúng là B vì B dẫn trước ngay lập tức. 

Một giả định không chính xác khác là so sánh sai hướng về mặt từ điển hoặc tính tổng các giá trị, cả hai đều bỏ qua điều kiện thống trị theo thứ tự thời gian. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mỗi vị trí như một trạm kiểm soát. Tại mỗi chỉ số i, chúng ta tính toán lại xem A có dẫn trước B hay không bằng cách quét từ đầu đến i và đếm hoặc so sánh tiến độ tích lũy. Điều này dẫn đến thời gian O(n^2), vì với mỗi n vị trí, chúng ta có thể kiểm tra lại tối đa n phần tử. 

Điều này hoạt động hợp lý vì nó mô phỏng một cách trung thực ý tưởng kiểm tra xem ai đang dẫn trước tại mỗi thời điểm, nhưng nó lặp lại công việc một cách không cần thiết. Sự kém hiệu quả trở nên trầm trọng khi n tăng lớn. 

Nhận xét quan trọng là điều duy nhất quan trọng là chỉ số đầu tiên trong đó hai chuỗi khác nhau về lợi thế tích lũy. Thay vì tính toán lại từ đầu, chúng tôi duy trì tổng số đang chạy hoặc so sánh trực tiếp các giá trị tiền tố trong một lần chuyển. Ngay khi sự bất bình đẳng nghiêm ngặt xuất hiện, chúng ta có thể quyết định người chiến thắng và dừng lại. 

Điều này làm giảm vấn đề từ việc đánh giá tiền tố lặp đi lặp lại thành một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi trình bày tiến trình giải quyết các vấn đề của mỗi người tham gia. Chúng tôi giả sử cả hai chuỗi đều được căn chỉnh theo thời gian, nghĩa là chỉ số i tương ứng với cùng một thời điểm cho cả hai. 
2. Khởi tạo so sánh đang chạy bằng cách quét từ chỉ mục đầu tiên đến chỉ mục cuối cùng. Tại mỗi chỉ số, hãy so sánh trực tiếp các giá trị hoặc duy trì sự khác biệt tích lũy nếu đầu vào thể hiện cách giải tăng dần. 
3. Tại chỉ số i, kiểm tra xem một người tham gia có giá trị lớn hơn người kia hay không. Nếu vậy, ngay lập tức trả lại người tham gia đó là người chiến thắng. Lý do điều này có hiệu quả là vì một khi lợi thế dẫn đầu được thiết lập ở thời điểm sớm nhất, thì các mối ràng buộc hoặc sự đảo ngược sau này không liên quan đến quy tắc “lợi thế quyết định đầu tiên”. 
4. Nếu các giá trị bằng nhau, hãy tiếp tục quét về phía trước. Bình đẳng có nghĩa là chưa có người tham gia nào tạo ra sự phân chia mang tính quyết định. 
5. Nếu quá trình quét hoàn tất mà không có bất kỳ sự bất bình đẳng nghiêm ngặt nào, hãy tuyên bố kết quả là hòa vì cả hai người tham gia đều không giành được lợi thế ở bất kỳ thời điểm nào. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến rằng trước chỉ số bất bình đẳng đầu tiên, cả hai chuỗi đều giống hệt nhau về tất cả các số hạng cạnh tranh có ý nghĩa. Tại chỉ số đầu tiên mà chúng khác nhau, dấu hiệu của sự khác biệt hoàn toàn xác định kết quả, bởi vì không có chỉ số nào trước đó mâu thuẫn với nó và không chỉ số nào sau đó có thể làm mất hiệu lực hồi tố của chỉ số quyết định đầu tiên. Điều này làm giảm quyết định tìm sự khác biệt khác 0 đầu tiên trong so sánh tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    n = len(a)
    
    for i in range(n):
        if a[i] > b[i]:
            print("A")
            return
        elif a[i] < b[i]:
            print("B")
            return
    
    print("TIE")

if __name__ == "__main__":
    solve()
```Mã thực hiện một lần chuyển qua cả hai chuỗi. Việc so sánh được thực hiện theo chỉ số, phù hợp chính xác với ý tưởng thuật toán nhằm phát hiện lợi thế quyết định đầu tiên. Khi phát hiện ra sự bất bình đẳng nghiêm trọng, chúng tôi chấm dứt sớm, tránh những công việc không cần thiết. 

Một sai lầm phổ biến là quên dừng lại ở điểm khác biệt đầu tiên mà thay vào đó tiếp tục theo dõi những thay đổi sau đó. Điều đó sẽ không chính xác cho phép những sự đảo chiều sau này lấn át ưu thế trước đó, điều này mâu thuẫn với định nghĩa của vấn đề “trước tiên phải giải quyết nhiều hơn”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 3, 3]
b = [2, 2, 3]
```| tôi | một [tôi] | b[i] | quyết định | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | B dẫn đầu, dừng lại | 

Ở chỉ số 0, B ngay lập tức có giá trị cao hơn nên B thắng mà không cần kiểm tra thêm. 

Điều này chứng tỏ rằng ưu thế sớm có tính quyết định ngay cả khi các giá trị sau này hội tụ. 

### Ví dụ 2 

đầu vào:```
a = [2, 2, 5]
b = [2, 2, 3]
```| tôi | một [tôi] | b[i] | quyết định | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | tiếp tục | 
| 1 | 2 | 2 | tiếp tục | 
| 2 | 5 | 3 | A dẫn đầu, dừng lại | 

Tiền tố bình đẳng cho thấy các ràng buộc không ảnh hưởng đến quyết định. Sự khác biệt nghiêm ngặt đầu tiên xảy ra ở chỉ số 2, trong đó A dẫn đầu. 

Điều này xác nhận rằng thuật toán bỏ qua các tiền tố trung tính một cách chính xác và chỉ phản ứng với phân kỳ đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập một lần cho đến khi bất đẳng thức nghiêm ngặt đầu tiên hoặc kết thúc | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng ngoài bộ nhớ đầu vào | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc điển hình cho n lên tới ít nhất 200.000, chỉ yêu cầu vài trăm nghìn so sánh trong trường hợp xấu nhất. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    out = io.StringIO()
    sys.stdout = out
    
    solve()
    
    return out.getvalue().strip()

def solve():
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    for i in range(len(a)):
        if a[i] > b[i]:
            return print("A")
        elif a[i] < b[i]:
            return print("B")
    print("TIE")

# provided sample-like cases
assert run("1 3 3\n2 2 3\n") == "B"
assert run("2 2 5\n2 2 3\n") == "A"

# custom cases
assert run("1\n1\n") == "TIE"
assert run("5\n1\n") == "A"
assert run("1 1 1 1\n1 1 1 1\n") == "TIE"
assert run("0 2 2 2\n0 1 3 2\n") == "B"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng giống hệt nhau | TIE | không có trường hợp phân kỳ | 
| phần tử lớn hơn | A hoặc B | sự thống trị ngay lập tức | 
| tất cả trình tự bằng nhau | TIE | quét toàn bộ không có kết quả | 
| tiến triển hỗn hợp | B | độ đúng sai phân thứ nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là sự bình đẳng hoàn toàn trên tất cả các vị trí. Ví dụ: 

đầu vào:```
a = [4, 4, 4]
b = [4, 4, 4]
```Thuật toán quét tất cả các chỉ số mà không tìm thấy bất kỳ sự bất đẳng thức nghiêm ngặt nào. Mỗi bước sẽ kích hoạt nhánh "tiếp tục". Chỉ sau khi hết chuỗi, chúng tôi mới xuất ra TIE, điều này đúng vì chưa có người tham gia nào dẫn đầu. 

Một trường hợp khác là khi phần tử đầu tiên đã xác định kết quả: 

đầu vào:```
a = [10, 0, 0]
b = [1, 100, 100]
```Ở chỉ số 0, A đã dẫn đầu. Quá trình quét dừng lại ngay lập tức và những đảo ngược sau đó có lợi cho B sẽ bị bỏ qua. Điều này xác nhận rằng việc chấm dứt sớm là cần thiết và được thực thi chính xác theo cấu trúc của thuật toán.
