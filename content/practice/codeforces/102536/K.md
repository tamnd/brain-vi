---
title: "CF 102536K - Tôi Brook Mã!"
description: "Đầu vào mô tả cùng một nhóm người trong hai mảng song song. Giá trị tại vị trí i trong mảng trọng số thuộc về người có chiều cao được lưu ở vị trí i trong mảng chiều cao."
date: "2026-08-04T02:14:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "K"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 123
verified: true
draft: false
---

[CF 102536K - Tôi Brook the Code!](https://codeforces.com/problemset/problem/102536/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả cùng một nhóm người trong hai mảng song song. Giá trị tại vị trí`i`trong mảng trọng số thuộc về người có chiều cao được lưu tại vị trí`i`trong mảng độ cao. Mục tiêu là khôi phục trình tự ẩn bằng cách sắp xếp những người này từ chiều cao thấp nhất đến chiều cao cao nhất và in trọng lượng của họ theo thứ tự mới đó. 

Chi tiết quan trọng là độ cao xác định thứ tự, trong khi trọng số là giá trị chúng ta cần di chuyển. Hai mảng không thể được sắp xếp độc lập vì mối quan hệ giữa chiều cao và cân nặng của một người phải đi cùng nhau. một cặp`(height, weight)`đại diện cho một người và câu trả lời là danh sách trọng số sau khi sắp xếp các cặp này theo chiều cao. 

Ràng buộc`n <= 100000`có nghĩa là giải pháp cần xử lý khoảng một trăm nghìn người một cách hiệu quả. Một thuật toán so sánh từng cặp người sẽ yêu cầu khoảng mười tỷ phép so sánh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Một cách tiếp cận dựa trên sắp xếp với`O(n log n)`hoạt động phù hợp vì nó thực hiện khoảng vài triệu so sánh cho kích thước đầu vào này. 

Các giá trị lớn về trọng lượng và chiều cao, lên tới`10^11`, loại trừ các phương pháp phụ thuộc vào phạm vi nhỏ chẳng hạn như sắp xếp đếm. Họ cũng yêu cầu sử dụng các loại số nguyên có thể lưu trữ các giá trị lớn. Số nguyên Python đã hỗ trợ độ chính xác tùy ý, do đó không cần xử lý đặc biệt. 

Một lỗi phổ biến là chỉ sắp xếp trọng số hoặc chỉ chiều cao. Ví dụ, hãy xem xét:```
3
5 1 9
2 1 3
```Đầu ra đúng là:```
1 5 9
```Những người được sắp xếp theo chiều cao là`(height=1, weight=1)`,`(height=2, weight=5)`, Và`(height=3, weight=9)`. Nếu ai đó sắp xếp các trọng số một cách độc lập, họ sẽ vô tình làm mất thông tin cặp ban đầu. 

Một trường hợp khác là khi đầu vào đã được sắp xếp theo chiều cao:```
3
7 8 9
10 20 30
```Đầu ra vẫn còn:```
7 8 9
```Một giải pháp giả định rằng một số sự sắp xếp lại phải xảy ra có thể sửa đổi câu trả lời một cách không chính xác. 

Một người duy nhất cũng là một trường hợp hợp lệ:```
1
42
100000000000
```Câu trả lời là:```
42
```Không cần thực hiện công việc sắp xếp nào ngoài việc xử lý một cặp hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liên tục tìm ra người có chiều cao còn lại nhỏ nhất, đưa ra cân nặng của họ và loại họ khỏi danh sách xem xét. Điều này đúng vì mỗi lựa chọn sẽ chọn chính xác người tiếp theo theo thứ tự chiều cao. Tuy nhiên, việc tìm ra mức tối thiểu trong số những người còn lại tốn thời gian tuyến tính và thực hiện việc đó cho tất cả mọi người.`n`chi phí con người:$$n + (n-1) + (n-2) + \dots + 1 = O(n^2)$$Vì`n = 100000`, việc này đạt tới khoảng năm tỷ so sánh và không thể hoàn thành trong thời gian giới hạn. 

Quan sát làm cho vấn đề trở nên đơn giản là thao tác duy nhất cần thực hiện là sắp xếp mọi người theo một trường trong khi mang theo một trường khác. Thay vì tìm kiếm chiều cao nhỏ nhất tiếp theo theo cách thủ công, chúng ta có thể để sắp xếp so sánh thực hiện tất cả các quyết định đặt hàng. Chúng tôi tạo các cặp chứa chiều cao và cân nặng của mỗi người, sắp xếp các cặp theo chiều cao và đọc cân nặng theo thứ tự được sắp xếp. 

Phương pháp brute-force hoạt động vì nó xây dựng lại thứ tự đã sắp xếp cho từng người một, nhưng nó thất bại vì nó liên tục giải quyết cùng một vấn đề về thứ tự. Cách tiếp cận sắp xếp giải quyết tất cả các quyết định sắp xếp cùng nhau, giảm độ phức tạp từ bậc hai xuống`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo danh sách các cặp trong đó mỗi cặp lưu trữ chiều cao và cân nặng của một người. Chiều cao là chìa khóa sắp xếp, trong khi trọng lượng là thông tin phải được phục hồi sau khi sắp xếp. 
2. Sắp xếp danh sách các cặp theo giá trị chiều cao theo thứ tự tăng dần. Vì mỗi chiều cao là duy nhất nên điều này tạo ra chính xác một thứ tự người hợp lệ. 
3. Duyệt qua các cặp đã được sắp xếp và thu thập giá trị trọng số của chúng. Trình tự được thu thập là mã ẩn vì nó khớp với thứ tự mà John sẽ nhận được sau khi sắp xếp mọi người theo chiều cao. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến mà mọi cặp vẫn được kết nối trong suốt quá trình. Việc sắp xếp chỉ thay đổi vị trí của những người hoàn chỉnh chứ không thay đổi mối quan hệ giữa chiều cao và cân nặng tương ứng. Sau khi sắp xếp, cặp thứ nhất có chiều cao nhỏ nhất, cặp thứ hai có chiều cao nhỏ thứ hai, v.v. Do đó, việc đọc trọng số từ các cặp này sẽ tạo ra chính xác trình tự được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    weights = list(map(int, input().split()))
    heights = list(map(int, input().split()))

    people = [(heights[i], weights[i]) for i in range(n)]
    people.sort()

    answer = [str(weight) for height, weight in people]
    print(" ".join(answer))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ đọc cả hai mảng một cách riêng biệt vì đầu vào lưu trữ chiều cao và trọng lượng ở các vị trí song song. Việc hiểu danh sách tạo ra`(height, weight)`cặp để việc sắp xếp giữ dữ liệu của mỗi người với nhau. 

Cuộc gọi tới`sort()`sử dụng thứ tự tuple của Python. Nó so sánh phần tử đầu tiên của mỗi bộ dữ liệu, đó là chiều cao và đặt các chiều cao nhỏ hơn trước đó. Vấn đề đảm bảo tất cả các độ cao đều khác nhau, do đó không có so sánh thứ cấp nào có thể ảnh hưởng đến kết quả. 

Vòng lặp cuối cùng chỉ trích xuất trọng số từ mỗi cặp được sắp xếp. Xây dựng chuỗi trước khi nối sẽ tránh việc nối chuỗi lặp lại và giữ cho đầu ra hiệu quả cho`100000`các giá trị. 

Số nguyên Python xử lý các giá trị lên đến`10^11`không tràn. Không có trường hợp cạnh lập chỉ mục nào vì mọi chỉ mục từ`0`ĐẾN`n-1`được sử dụng chính xác một lần khi xây dựng các cặp. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3
2 1 4
1 4 3
```các cặp là`(1,2)`,`(4,1)`, Và`(3,4)`. 

| Bước | Sắp xếp cặp | Trích câu trả lời | 
| --- | --- | --- | 
| Cặp ban đầu | (1,2), (4,1), (3,4) | trống | 
| Sau khi sắp xếp | (1,2), (3,4), (4,1) | trống | 
| Đọc trọng lượng | (1,2), (3,4), (4,1) | 2 4 1 | 

Dấu vết cho thấy tại sao việc lưu trữ các cặp là cần thiết. Các quả nặng chuyển động vì độ cao tương ứng của chúng chuyển động. 

Một ví dụ thứ hai:```
4
30 10 40 20
50 10 40 30
```Các cặp là`(50,30)`,`(10,10)`,`(40,40)`, Và`(30,20)`. 

| Bước | Sắp xếp cặp | Trích câu trả lời | 
| --- | --- | --- | 
| Cặp ban đầu | (50,30), (10,10), (40,40), (30,20) | trống | 
| Sau khi sắp xếp | (10,10), (30,20), (40,40), (50,30) | trống | 
| Đọc trọng lượng | (10,10), (30,20), (40,40), (50,30) | 10 20 40 30 | 

Ví dụ này thực hiện thứ tự nhập được xáo trộn hoàn toàn và xác nhận rằng vị trí ban đầu không quan trọng sau khi ghép chiều cao với cân nặng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp`n`cặp chiều cao-cân nặng chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(n) | Danh sách cặp và kho lưu trữ câu trả lời đều tăng tuyến tính theo số lượng người. | 

Vì`n = 100000`, việc sắp xếp yêu cầu số lượng so sánh có thể quản lý được và phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính và duy trì trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    weights = list(map(int, sys.stdin.readline().split()))
    heights = list(map(int, sys.stdin.readline().split()))

    people = [(heights[i], weights[i]) for i in range(n)]
    people.sort()

    result = " ".join(str(weight) for height, weight in people)

    sys.stdin = old_stdin
    return result

assert solve("""3
2 1 4
1 4 3
""") == "2 4 1", "sample 1"

assert solve("""1
42
100000000000
""") == "42", "single person"

assert solve("""4
30 10 40 20
50 10 40 30
""") == "10 20 40 30", "unordered heights"

assert solve("""5
8 8 8 8 8
5 1 4 3 2
""") == "8 8 8 8 8", "equal weights"

assert solve("""5
100 200 300 400 500
50000000000 1 99999999999 2 3
""") == "200 400 500 100 300", "large height values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`với một cặp |`42`| Xử lý đầu vào nhỏ nhất có thể. | 
| Độ cao xáo trộn |`10 20 40 30`| Xác nhận việc sắp xếp dựa trên chiều cao chứ không phải thứ tự đầu vào. | 
| Tất cả trọng lượng bằng nhau |`8 8 8 8 8`| Xác nhận các giá trị bằng nhau không ảnh hưởng đến việc đặt hàng. | 
| Độ cao rất lớn |`200 400 500 100 300`| Kiểm tra việc xử lý các giá trị gần giới hạn trên. | 

## Vỏ cạnh 

Đối với trường hợp chỉ sắp xếp một mảng làm mất mối quan hệ giữa các giá trị, hãy xem xét:```
3
5 1 9
2 1 3
```Thuật toán tạo ra các cặp`(2,5)`,`(1,1)`, Và`(3,9)`. Sắp xếp mang lại`(1,1)`,`(2,5)`, Và`(3,9)`, vì vậy đầu ra là`1 5 9`. Các giá trị trọng số luôn được gắn với đúng người, tránh sai lầm khi sắp xếp các mảng một cách độc lập. 

Đối với trường hợp đã được sắp xếp:```
3
7 8 9
10 20 30
```Các cặp là`(10,7)`,`(20,8)`, Và`(30,9)`. Việc sắp xếp không làm thay đổi thứ tự của chúng và việc trích xuất trọng số sẽ cho`7 8 9`. Thuật toán không cho rằng chuyển động là cần thiết. 

Đối với trường hợp một người:```
1
42
100000000000
```Danh sách cặp chỉ chứa`(100000000000,42)`. Việc sắp xếp danh sách một phần tử không thay đổi và trọng số được trích xuất là`42`, phù hợp với đầu ra được yêu cầu.
