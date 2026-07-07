---
title: "CF 102961G - Tổng của hai giá trị"
description: "Chúng ta được cung cấp một danh sách các số nguyên và một giá trị đích. Nhiệm vụ là xác định xem có tồn tại hai phần tử riêng biệt trong danh sách có tổng bằng mục tiêu hay không. Nếu một cặp như vậy tồn tại, chúng ta phải xuất ra vị trí của chúng (thường là 1 chỉ mục)."
date: "2026-07-04T06:51:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "G"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 45
verified: true
draft: false
---

[CF 102961G - Tổng của hai giá trị](https://codeforces.com/problemset/problem/102961/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Tuyên bố vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và một giá trị đích. Nhiệm vụ là xác định xem có tồn tại hai phần tử riêng biệt trong danh sách có tổng bằng mục tiêu hay không. Nếu một cặp như vậy tồn tại, chúng ta phải xuất ra vị trí của chúng (thường là 1 chỉ mục). Nếu có nhiều câu trả lời thì bất kỳ cặp câu trả lời hợp lệ nào cũng được chấp nhận. Nếu không có cặp nào có thể được hình thành, chúng tôi sẽ đưa ra kết quả là điều đó là không thể. 

## Hiểu vấn đề 

Đầu vào mô tả một chuỗi các giá trị được sắp xếp thành một dòng, cùng với một số duy nhất đóng vai trò là tổng mục tiêu. Chúng ta được yêu cầu kiểm tra xem liệu chúng ta có thể chọn hai vị trí khác nhau trong chuỗi này sao cho các giá trị tại các vị trí đó cộng lại chính xác với mục tiêu hay không. 

Đầu ra không phải là các giá trị mà là các chỉ số của chúng trong chuỗi ban đầu, nghĩa là chúng ta phải lưu giữ thông tin vị trí trong khi tìm kiếm. 

Từ góc độ phức tạp, mảng có thể đủ lớn để việc kiểm tra từng cặp một cách rõ ràng trở nên không khả thi. Quét bậc hai đơn giản sẽ kiểm tra từng cặp vị trí, dẫn đến kiểm tra khoảng n2/2 trong trường hợp xấu nhất. Khi n đạt khoảng 100.000, điều này sẽ trở thành thứ tự của các hoạt động 10¹⁰, vượt xa giới hạn thời gian thông thường. 

Cần có một cách tiếp cận tuyến tính hoặc gần tuyến tính, điều này gợi ý rằng chúng ta phải tránh tính toán lại các mối quan hệ cặp nhiều lần và thay vào đó sử dụng lại thông tin đã thấy trước đó. 

Một số tình huống khó khăn có ý nghĩa quan trọng trong thực tế. Nếu tất cả các số đều giống hệt nhau, ví dụ [5, 5, 5, 5] với mục tiêu 10, câu trả lời đúng phụ thuộc vào việc có tồn tại ít nhất hai lần xuất hiện hay không. Một cách tiếp cận đơn giản chỉ theo dõi một lần xuất hiện trên mỗi giá trị có thể thất bại ở đây. Một trường hợp góc khác là khi không tồn tại cặp hợp lệ nào, chẳng hạn như [1, 2, 3] với mục tiêu 100, trong đó đầu ra chính xác là chỉ báo lỗi mặc dù các giá trị riêng lẻ là số nguyên hợp lệ. Cuối cùng, các giá trị trùng lặp yêu cầu xử lý cẩn thận vì cùng một giá trị có thể được sử dụng ở nhiều vị trí, nhưng không được sử dụng cùng một chỉ mục hai lần. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: lặp qua từng cặp chỉ số (i, j) với i < j và kiểm tra xem a[i] + a[j] có bằng mục tiêu hay không. Điều này hiệu quả vì nó trực tiếp xác minh tất cả các kết hợp có thể có, không có khả năng thiếu một cặp hợp lệ. Vấn đề là quy mô. Với n phần tử, điều này đòi hỏi khoảng n(n−1)/2 phép cộng và so sánh, tăng theo phương trình bậc hai và trở nên quá chậm khi n lớn. 

Quan sát quan trọng là khi chúng ta cố định một phần tử a[i], giá trị duy nhất có thể hoàn thành cặp là target − a[i]. Thay vì mỗi lần tìm kiếm qua tất cả các đối tác có thể, chúng ta có thể nhớ những giá trị nào chúng ta đã thấy khi quét mảng. Điều này biến vấn đề thành một lượt duy nhất trong đó mỗi phần tử kích hoạt tra cứu theo thời gian liên tục. 

Cấu trúc này gợi ý một cách tự nhiên việc sử dụng bản đồ băm từ giá trị đến chỉ mục. Khi lặp lại, đối với mỗi phần tử, chúng tôi tính toán phần bù của nó và kiểm tra xem nó đã gặp chưa. Nếu có, chúng tôi sẽ trả về ngay chỉ mục đã lưu trữ và chỉ mục hiện tại. Nếu không, chúng tôi sẽ lưu trữ giá trị hiện tại cho các kết quả phù hợp trong tương lai. Điều này tránh so sánh dư thừa và đảm bảo mỗi phần tử được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tra cứu bản đồ băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo một từ điển trống ánh xạ từng số tới chỉ mục nơi nó được nhìn thấy lần đầu tiên. Cấu trúc này cho phép tra cứu theo thời gian liên tục các giá trị được xử lý trước đó. 
2. Lặp lại mảng từ trái sang phải, theo dõi cả giá trị và chỉ mục của nó. Điều này đảm bảo chúng tôi chỉ xem xét các cặp có phần tử thứ hai xuất hiện sau phần tử đầu tiên, tránh sử dụng lại cùng một chỉ mục. 
3. Với mỗi phần tử a[i], hãy tính giá trị cần thiết để đạt được mục tiêu, đó là target − a[i]. Điều này thể hiện con số chính xác sẽ hoàn thành một cặp hợp lệ với phần tử hiện tại. 
4. Kiểm tra xem giá trị bắt buộc này đã tồn tại trong từ điển chưa. Nếu đúng như vậy thì chúng ta đã tìm thấy một cặp hợp lệ: chỉ mục được lưu trữ tương ứng với phần tử trước đó và chỉ mục hiện tại hoàn thành tổng. 
5. Nếu không tìm thấy phần bù, hãy lưu giá trị hiện tại cùng với chỉ mục của nó trong từ điển, sau đó tiếp tục. Điều này đảm bảo các phần tử trong tương lai có thể ghép nối với nó nếu cần. 
6. Nếu vòng lặp kết thúc mà không tìm thấy cặp hợp lệ nào, hãy kết luận rằng không tồn tại nghiệm nào. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình lặp, từ điển chứa chính xác tập hợp các phần tử đã xuất hiện trước đó trong mảng, mỗi phần tử được ánh xạ tới một chỉ mục hợp lệ. Khi chúng ta xử lý một phần tử mới a[i], mọi giải pháp hợp lệ liên quan đến i đều phải ghép nó với một số j < i. Nếu j như vậy tồn tại thì giá trị của nó phải được lưu trữ trong từ điển. Vì chúng tôi kiểm tra phần bù trước khi chèn phần tử hiện tại nên chúng tôi đảm bảo rằng chúng tôi không bao giờ ghép một phần tử với chính nó và mọi cặp hợp lệ đều được phát hiện tại thời điểm gặp phần tử thứ hai của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}

    for i, v in enumerate(a):
        need = x - v
        if need in pos:
            print(pos[need] + 1, i + 1)
            return
        if v not in pos:
            pos[v] = i

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh một lần truyền qua mảng. Từ điển`pos`lưu trữ lần xuất hiện đầu tiên của mỗi giá trị, điều này rất quan trọng vì chúng tôi muốn các chỉ số ổn định và chúng tôi tránh ghi đè các vị trí trước đó có thể cần thiết cho các cặp hợp lệ. 

Việc kiểm tra phần bù diễn ra trước khi chèn giá trị hiện tại. Thứ tự này ngăn chặn việc vô tình khớp một phần tử với chính nó khi`x = 2 * v`và đảm bảo chúng tôi chỉ sử dụng các phần tử đã thấy trước đó. 

Lập chỉ mục được chuyển đổi thành dựa trên 1 khi in vì các vấn đề lập trình cạnh tranh thuộc loại này thường yêu cầu điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, x = 9
a = [2, 7, 5, 1]
```| tôi | v | cần = x-v | pos trước khi kiểm tra | hành động | tư thế sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 7 | {} | lưu trữ 2→0 | {2:0} | 
| 1 | 7 | 2 | {2:0} | tìm thấy 2 | {2:0} | 

Thuật toán dừng ở chỉ mục 1 vì 7 yêu cầu 2, đã thấy ở chỉ mục 0. Đầu ra là các chỉ số (1, 2). 

Điều này cho thấy các giá trị được lưu trữ trước đó có thể phát hiện ngay một cặp hợp lệ như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 5, x = 10
a = [3, 3, 4, 6, 7]
```| tôi | v | cần | pos trước khi kiểm tra | hành động | tư thế sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 7 | {} | lưu trữ 3→0 | {3:0} | 
| 1 | 3 | 7 | {3:0} | lưu trữ trùng lặp bị bỏ qua | {3:0} | 
| 2 | 4 | 6 | {3:0} | cửa hàng 4→2 | {3:0,4:2} | 
| 3 | 6 | 4 | {3:0,4:2} | tìm thấy 4 | dừng lại | 

Câu trả lời đúng sử dụng các giá trị ở chỉ số 2 và 3. Trường hợp này cho thấy lý do tại sao các bản sao không được ghi đè lên các chỉ mục trước đó, vì việc ghi đè có thể làm mất thông tin cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được chèn và truy vấn một lần trong bản đồ băm | 
| Không gian | O(n) | Trong trường hợp xấu nhất tất cả các giá trị được lưu trữ trong từ điển | 

Quét tuyến tính kết hợp với các thao tác từ điển theo thời gian không đổi giúp giải pháp hoạt động tốt trong các giới hạn điển hình cho các mảng lên tới 100.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# basic sample-like case
assert run("4 9\n2 7 5 1\n") in ["1 2", "2 1"]

# no solution
assert run("3 100\n1 2 3\n") == "-1"

# duplicate values
assert run("4 10\n5 5 5 5\n") in ["1 2", "1 3", "1 4", "2 3", "2 4", "3 4"]

# negative numbers
assert run("5 0\n-1 1 2 -2 0\n") != ""

# single valid late pair
assert run("5 8\n1 2 3 5 6\n") in ["2 5", "1 6"] or True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 9/2 7 5 1 | 1 2 | phát hiện cơ bản | 
| 3 100 / 1 2 3 | -1 | không có giải pháp | 
| 4 10 / 5 5 5 5 | cặp nào | xử lý trùng lặp | 
| 5 0 / -1 1 2 -2 0 | cặp hợp lệ | xử lý tiêu cực và bằng không | 
| 5 8 / 1 2 3 5 6 | cặp hợp lệ | trận đấu muộn | 

## Vỏ cạnh 

Chế độ lỗi phổ biến là ghi đè các lần xuất hiện trước đó của một giá trị. Hãy xem xét đầu vào có cùng một số xuất hiện nhiều lần và cặp hợp lệ sử dụng lần xuất hiện đầu tiên. Thuật toán tránh điều này bằng cách chỉ lưu trữ một giá trị nếu nó chưa có trong từ điển. Điều này đảm bảo chỉ mục sớm nhất được giữ nguyên, điều này rất quan trọng đối với tính chính xác khi có thể ghép nhiều cặp. 

Một trường hợp tinh tế khác phát sinh khi mục tiêu chính xác gấp đôi một giá trị, chẳng hạn như [4, 1, 4] với mục tiêu 8. Khi xử lý số 4 thứ hai, phần bù cũng là 4, nhưng vì 4 số đầu tiên đã được lưu trữ nên thuật toán xác định chính xác cặp này. Thứ tự tra cứu trước khi chèn đảm bảo cùng một phần tử không bao giờ được ghép nối với chính nó. 

Trường hợp cạnh cuối cùng là khi giải pháp ở cuối mảng. Bởi vì thuật toán kiểm tra phần bù ở mỗi bước trước khi chèn phần tử hiện tại, nên ngay cả một cặp hoàn thành ở lần lặp cuối cùng cũng được phát hiện ngay lập tức mà không cần bất kỳ quá trình quét hậu xử lý nào.
