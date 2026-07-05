---
title: "CF 102897J - \u5927\u626b\u9664"
description: "Chúng tôi được cung cấp một số mô tả tòa nhà độc lập. Mỗi tòa nhà bao gồm nhiều tầng và mỗi tầng được biểu thị bằng một chuỗi. Các ký tự trong chuỗi mô tả vị trí có chứa thùng rác hay không và nó thuộc loại gì."
date: "2026-07-04T08:48:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "J"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 35
verified: true
draft: false
---

[CF 102897J - \u5927\u626b\u9664](https://codeforces.com/problemset/problem/102897/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mô tả tòa nhà độc lập. Mỗi tòa nhà bao gồm nhiều tầng và mỗi tầng được biểu thị bằng một chuỗi. Các ký tự trong chuỗi mô tả vị trí có chứa thùng rác hay không và nó thuộc loại gì. Ký tự dấu chấm đại diện cho một vị trí trống, trong khi bất kỳ ký tự nào khác đại diện cho mục rác và các ký tự khác nhau tương ứng với các loại rác khác nhau. 

Đối với mỗi tầng, chúng tôi được yêu cầu đếm xem có bao nhiêu loại rác riêng biệt xuất hiện trên tầng đó. Sau đó, chúng tôi tính tổng giá trị này trên tất cả các tầng trong tòa nhà. Cuối cùng, chúng tôi xuất ra tổng số này cho từng trường hợp thử nghiệm. 

Điểm mấu chốt là sự khác biệt được đo lường độc lập trên mỗi tầng. Nếu cùng một loại rác xuất hiện trên nhiều tầng, nó sẽ đóng góp riêng biệt ở từng tầng nơi nó xuất hiện. Nếu một tầng có nhiều lần xuất hiện của cùng một ký tự thì nó vẫn chỉ đóng góp một lần cho tầng đó. 

Các ràng buộc ngụ ý một giải pháp quét tuyến tính. Tổng số ký tự trên tất cả các tầng trong tệp thử nghiệm có thể lên tới 10^7. Điều này ngay lập tức loại trừ mọi cấu trúc tốn kém cho mỗi ký tự như sắp xếp chuỗi con hoặc khởi tạo lại tập hợp lặp lại với chi phí cao cho mỗi truy vấn. Chúng tôi cần một cách tiếp cận xử lý từng ký tự về cơ bản một lần, với các bản cập nhật liên tục. 

Một trường hợp thất bại nhỏ xuất hiện khi người ta tổng hợp nhầm các loại rác trên toàn bộ tòa nhà thay vì trên mỗi tầng. Ví dụ, hãy xem xét: 

Tầng 1:`a.a`Tầng 2:`aa.`Giải thích đúng: 

Tầng 1 có`{a}`vì vậy đóng góp 1. 

Tầng 2 còn có`{a}`vì vậy đóng góp 1. 

Câu trả lời là 2. 

Một cách tiếp cận tập hợp toàn cầu sai sẽ coi nó là một loại tổng và đầu ra 1. 

Một trường hợp tinh vi khác là các ký tự lặp lại trên cùng một tầng. Ví dụ:`####`Điều này sẽ đóng góp 1, không phải 4. 

## Phương pháp tiếp cận 

Cách giải thích đơn giản là xử lý từng tầng một cách độc lập, xây dựng một tập hợp tất cả các ký tự không có dấu chấm trong chuỗi đó và thêm kích thước của nó vào câu trả lời. Điều này đúng vì một bộ thực thi một cách tự nhiên tính duy nhất của các loại rác trên mỗi tầng. 

Tuy nhiên, mối quan tâm ngây thơ là hiệu suất. Nếu chúng ta tạo một bộ mới và chèn từng ký tự theo đúng nghĩa đen thì chúng ta vẫn chạm vào từng ký tự một lần, do đó độ phức tạp là tuyến tính trong tổng kích thước đầu vào. Chi phí tiềm năng duy nhất là các hoạt động tập hợp hàm băm, nhưng trong Python, điều này vẫn có thể chấp nhận được với giới hạn tổng số 10^7 ký tự. 

Chúng tôi có thể tinh chỉnh việc triển khai bằng cách tránh phân bổ lặp lại hoặc tạo đối tượng nặng nếu có thể. Thay vì lưu trữ rõ ràng các bộ đầy đủ cho mỗi tầng, chúng ta có thể duy trì một mảng đánh dấu boolean hoặc mảng dấu thời gian được nhìn thấy lần cuối cho từng loại ký tự. Đối với mỗi tầng, chúng tôi chỉ đặt lại các điểm đánh dấu mà chúng tôi đã sử dụng để tránh việc xóa các công trình lớn. 

Quan sát quan trọng là chúng ta không cần biết _tầng nào_ đã sử dụng một ký tự, chỉ cần biết liệu nó có xuất hiện ở tầng hiện tại hay không. Vì vậy, chúng ta có thể theo dõi các ký tự được nhìn thấy trên mỗi tầng trong hoạt động đánh dấu O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bộ mỗi tầng | O(tổng ký tự) | O(kích thước bảng chữ cái) | Đã chấp nhận | 
| Dấu thời gian/mảng boolean | O(tổng ký tự) | O(kích thước bảng chữ cái) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp kiểm tra là độc lập nên chúng tôi đặt lại câu trả lời tích lũy cho từng trường hợp. 
2. Đối với mỗi trường hợp thử nghiệm, hãy khởi tạo cấu trúc toàn cục`seen`ghi lại liệu một ký tự đã được tính ở tầng hiện tại hay chưa. Đây có thể là một từ điển hoặc mảng tùy thuộc vào các ràng buộc. 
3. Lặp lại từng chuỗi tầng. Trước khi xử lý một tầng mới, chỉ xóa hoặc đặt lại trạng thái liên quan đến tầng đó. Yêu cầu quan trọng là không được để thông tin từ các tầng trước rò rỉ sang tầng hiện tại. 
4. Quét chuỗi ký tự theo từng ký tự. Nếu ký tự là dấu chấm, hãy bỏ qua ngay vì nó không đại diện cho rác. 
5. Đối với bất kỳ ký tự không phải dấu chấm nào, hãy kiểm tra xem nó đã được tính cho tầng này chưa. Nếu không, hãy đánh dấu nó là đã xem và tăng mức đóng góp của tầng lên 1. Điều này đảm bảo mỗi loại rác đóng góp tối đa một lần cho mỗi tầng. 
6. Sau khi hoàn thành phần sàn, hãy cộng phần đóng góp của nó vào tổng đáp án. 
7. Chuyển sang tầng tiếp theo và lặp lại. 

### Tại sao nó hoạt động 

Mỗi tầng được coi như một vũ trụ độc lập nơi chúng tôi đếm các ký hiệu riêng biệt giữa các ký tự không có dấu chấm. Cấu trúc đánh dấu đảm bảo rằng mỗi biểu tượng được tính một lần trên mỗi tầng và việc đặt lại giữa các tầng đảm bảo tính độc lập. Vì mỗi ký tự được xử lý chính xác một lần và đóng góp nhiều nhất một mức tăng nên tổng cuối cùng khớp chính xác với định nghĩa “tổng các loại rác riêng biệt trên mỗi tầng”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        total = 0
        
        seen = set()
        
        for _ in range(n):
            s = input().strip()
            seen.clear()
            cnt = 0
            
            for ch in s:
                if ch == '.':
                    continue
                if ch not in seen:
                    seen.add(ch)
                    cnt += 1
            
            total += cnt
        
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng trường hợp kiểm thử một cách độc lập và tích lũy câu trả lời theo từng tầng. các`seen`bộ được dọn sạch cho từng tầng, đảm bảo không làm ô nhiễm các loại rác giữa các tầng. Mỗi ký tự được kiểm tra một lần và việc chèn vào bộ Python đảm bảo theo dõi tính duy nhất trong thời gian không đổi được khấu hao. 

Một cạm bẫy phổ biến là quên xóa`seen`giữa các tầng, điều này sẽ hợp nhất không chính xác các loại rác trên các tầng khác nhau và làm tăng số lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
..#.
#...
```| Tầng | Chuỗi | Đã thấy sự tiến hóa | Đóng góp sàn | 
| --- | --- | --- | --- | 
| 1 |`..#.`| {#, # đã là duy nhất} | 1 | 
| 2 |`#...`| {#} | 1 | 

Tổng số câu trả lời là 2. 

Điều này xác nhận rằng các loại rác giống nhau ở các tầng khác nhau sẽ được tính riêng. 

### Ví dụ 2 

đầu vào:```
1
####
#..#
```| Tầng | Chuỗi | Đã thấy sự tiến hóa | Đóng góp sàn | 
| --- | --- | --- | --- | 
| 1 |`####`| {#} | 1 | 
| 2 |`#..#`| {#} | 1 | 

Tổng số câu trả lời là 2. 

Điều này thể hiện sự trùng lặp trong một tầng: các ký hiệu lặp lại vẫn được tính một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ký tự) | Mỗi ký tự được xử lý một lần và kiểm tra thời gian trung bình O(1) trong một bộ | 
| Không gian | O(1) | Tối đa 26 hoặc giới hạn các ký hiệu riêng biệt được lưu trữ trên mỗi tầng | 

Tổng kích thước đầu vào lên tới 10^7 ký tự và thuật toán thực hiện lượng công việc không đổi trên mỗi ký tự, vừa vặn thoải mái trong giới hạn 1 giây thông thường trong Python được tối ưu hóa bằng các thao tác đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume solution is in solve()
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample style cases
assert run("""1
1
a""") == "1"

assert run("""1
2
a.a
aa.""") == "2"

# custom cases
assert run("""1
1
....""") == "0", "all empty"

assert run("""1
1
abcabc""") == "3", "dedup within floor"

assert run("""1
2
a
a""") == "2", "independent floors"

assert run("""1
3
a.b
.b.
aaa""") == "3", "mixed patterns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các dấu chấm | 0 | không xử lý rác | 
| ký tự lặp đi lặp lại | 3 | tính độc đáo của mỗi tầng | 
| cùng một char xuyên suốt các tầng | 2 | độc lập | 
| mẫu hỗn hợp | 3 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là sàn chỉ có dấu chấm chẳng hạn`"....."`. Thuật toán xóa`seen`, quét từng ký tự, không tìm thấy thùng rác hợp lệ và đóng góp 0. Tập hợp vẫn trống và đóng góp chính xác bằng 0. 

Một trường hợp khác là đầu vào một tầng cực kỳ dài bao gồm cùng một ký tự, chẳng hạn`"aaaaaaaa..."`. Lần xuất hiện đầu tiên sẽ chèn vào`seen`và tất cả các ký tự tiếp theo đều bị bỏ qua, tạo ra phần đóng góp là 1 bất kể độ dài. 

Trường hợp thứ ba là xen kẽ các tầng có ký hiệu chồng lên nhau, chẳng hạn như:```
a.b
b.a
```Ở tầng một,`seen`trở thành`{a, b}`và đóng góp 2. Trên tầng hai,`seen`được xóa và tính toán lại như`{a, b}`, đóng góp 2 lần nữa. Việc thiết lập lại đảm bảo không có hiện tượng lây lan, duy trì tính chính xác giữa các tầng.
