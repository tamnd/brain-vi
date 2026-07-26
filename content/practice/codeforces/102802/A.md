---
title: "CF 102802A - Chảo nướng"
description: "George có một bộ sưu tập các bánh quy hình tròn được đặt trên mặt phẳng tọa độ. Mỗi cookie được mô tả bằng tọa độ tâm và bán kính của nó. Anh ta cần một cái chảo nướng hình chữ nhật có các cạnh song song với các trục tọa độ và nó chứa hoàn toàn mọi chiếc bánh quy."
date: "2026-07-26T16:31:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102802
codeforces_index: "A"
codeforces_contest_name: "mBIT Varsity November 2019"
rating: 0
weight: 102802
solve_time_s: 42
verified: true
draft: false
---

[CF 102802A - Chảo nướng](https://codeforces.com/problemset/problem/102802/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

George có một bộ sưu tập các bánh quy hình tròn được đặt trên mặt phẳng tọa độ. Mỗi cookie được mô tả bằng tọa độ tâm và bán kính của nó. Anh ta cần một cái chảo nướng hình chữ nhật có các cạnh song song với các trục tọa độ và nó chứa hoàn toàn mọi chiếc bánh quy. Mục tiêu là tìm diện tích nhỏ nhất có thể của một chiếc chảo như vậy. 

Một cookie tập trung vào`(x, y)`với bán kính`r`đến từ`x - r`ĐẾN`x + r`theo chiều ngang và từ`y - r`ĐẾN`y + r`theo chiều dọc. Vì chảo được căn chỉnh theo trục nên kích thước ngang của nó chỉ phụ thuộc vào điểm ngoài cùng bên trái và ngoài cùng bên phải của tất cả các cookie và kích thước dọc của nó chỉ phụ thuộc vào điểm thấp nhất và cao nhất. 

Số lượng cookie có thể lớn tới mức`100000`, do đó lời giải không thể kiểm tra mọi cặp cookie có thể có hoặc thử các hình chữ nhật có thể có. Với giới hạn vài giây,`O(N^2)`phương pháp này sẽ thực hiện khoảng mười tỷ phép tính trong trường hợp xấu nhất, vượt xa những gì thực tế. Chúng tôi cần một đường chuyền duy nhất hoặc một cái gì đó gần như vậy. 

Các giá trị tọa độ và bán kính có thể đạt tới`10^7`về độ lớn. Chiều rộng và chiều cao cuối cùng có thể vào khoảng`4 * 10^7`, làm cho khu vực xung quanh`10^15`. Điều này có nghĩa là việc triển khai cần các loại số nguyên có thể chứa các sản phẩm lớn một cách an toàn. Số nguyên Python tự động xử lý việc này. 

Một số trường hợp nguy hiểm có thể phá vỡ các giải pháp bất cẩn. Nếu chỉ có một chiếc bánh quy, chảo phải vừa với đường kính của chiếc bánh quy đó theo cả hai hướng. Ví dụ:```
Input
1
0 0 5

Output
100
```Giải pháp chỉ xem xét khoảng cách giữa các cookie khác nhau sẽ không thành công vì không có cặp nào. 

Một lỗi phổ biến khác là quên rằng bán kính cookie sẽ mở rộng hộp giới hạn. Ví dụ:```
Input
2
0 0 1
2 0 1

Output
16
```Các cookie chiếm tọa độ ngang từ`-1`ĐẾN`3`, cho chiều rộng`4`và tọa độ dọc từ`-1`ĐẾN`1`, cho chiều cao`2`, vậy diện tích là`8`, không`16`. Việc triển khai bất cẩn sử dụng tâm làm góc hình chữ nhật sẽ tính sai kích thước. 

Đầu ra chính xác cho ví dụ trên thực sự là:```
8
```Hình chữ nhật chỉ cần che những chiếc bánh quy chứ không phải khoảng cách giữa các tâm của chúng sẽ tăng gấp đôi theo mọi hướng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình tìm hình chữ nhật bằng cách kiểm tra từng cookie và theo dõi tọa độ nhỏ nhất và lớn nhất hiện tại. Đây đã là ý tưởng thiết yếu. Một phiên bản đơn giản hơn có thể so sánh mọi cookie với mọi cookie khác để xác định các ranh giới cực đoan, nhưng điều đó thực hiện`O(N^2)`so sánh. Với`N = 100000`, điều này trở thành về`10^10`so sánh. 

Cấu trúc của bài toán cho chúng ta một quan sát đơn giản hơn. Hình chữ nhật cuối cùng được xác định hoàn toàn bởi bốn giá trị: tọa độ x nhỏ nhất mà bất kỳ cookie nào đạt được, tọa độ x lớn nhất mà bất kỳ cookie nào đạt được, tọa độ y nhỏ nhất mà bất kỳ cookie nào đạt được và tọa độ y lớn nhất mà bất kỳ cookie nào đạt được. Mỗi cookie chỉ đóng góp hai ranh giới x có thể có và hai ranh giới y có thể có. 

Cách tiếp cận bạo lực có hiệu quả vì câu trả lời phụ thuộc vào các điểm cực trị của tất cả các cookie, nhưng nó không thành công khi liên tục tìm kiếm các điểm cực trị đó. Quan sát thấy rằng mỗi cookie có thể cập nhật các giá trị cực đoan một cách độc lập cho phép chúng tôi giảm vấn đề xuống còn một lần quét qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến để biểu diễn hình chữ nhật giới hạn hiện tại. Đặt giá trị x và y tối thiểu thành một số rất lớn và đặt giá trị x và y tối đa thành một số rất nhỏ. Điều này tạo ra một hình chữ nhật trống sẽ mở rộng khi cookie được xử lý. 
2. Đọc từng cookie và tính bốn điểm cực trị của nó. Cạnh trái là`x - r`, cạnh phải là`x + r`, cạnh dưới là`y - r`, và cạnh trên là`y + r`. 
3. Cập nhật các giá trị tối thiểu và tối đa được lưu trữ bằng bốn ranh giới này. Lý do cập nhật trực tiếp là hình chữ nhật cuối cùng chỉ phụ thuộc vào vị trí đạt được xa nhất chứ không phụ thuộc vào vị trí của từng cookie. 
4. Sau khi xử lý xong mỗi cookie, hãy tính chiều rộng hình chữ nhật như sau:`max_x - min_x`và chiều cao như`max_y - min_y`. 
5. Nhân chiều rộng và chiều cao. Giá trị kết quả là diện tích chảo nhỏ nhất có thể vì bất kỳ hình chữ nhật nhỏ hơn nào cũng sẽ không chứa một trong các ranh giới cookie cực đoan. 

Tại sao nó hoạt động: 

Tại mọi thời điểm trong quá trình quét, các giá trị được lưu trữ mô tả hình chữ nhật thẳng hàng theo trục nhỏ nhất chứa tất cả các cookie được xử lý cho đến nay. Khi một cookie mới được thêm vào, toàn bộ diện tích của nó phải vừa với hình chữ nhật, vì vậy những thay đổi duy nhất có thể xảy ra là mở rộng một hoặc nhiều trong bốn cạnh. Sau khi tất cả cookie được xử lý, hình chữ nhật được lưu trữ sẽ chứa mọi cookie và mọi cạnh đều bị ép bởi ít nhất một ranh giới cookie, nghĩa là không bên nào có thể di chuyển vào trong. Một hình chữ nhật có cả bốn cạnh cố định theo cách này là cái chảo tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    min_x = float("inf")
    max_x = float("-inf")
    min_y = float("inf")
    max_y = float("-inf")
    
    for _ in range(n):
        x, y, r = map(int, input().split())
        
        min_x = min(min_x, x - r)
        max_x = max(max_x, x + r)
        min_y = min(min_y, y - r)
        max_y = max(max_y, y + r)
    
    print((max_x - min_x) * (max_y - min_y))

if __name__ == "__main__":
    solve()
```Chương trình duy trì bốn ranh giới được mô tả trong thuật toán. Mỗi cookie đầu vào ngay lập tức đóng góp các cạnh trái, phải, dưới và trên của nó. 

Các cập nhật diễn ra trong khi đọc dữ liệu đầu vào thay vì lưu trữ tất cả cookie. Điều này giúp duy trì mức sử dụng bộ nhớ liên tục và tránh những công việc không cần thiết. Phép nhân cuối cùng chỉ được thực hiện sau khi biết được hình chữ nhật giới hạn hoàn chỉnh. 

Không có phép tính dấu phẩy động mặc dù sử dụng vô cực để khởi tạo. Tọa độ và bán kính là số nguyên nên mọi ranh giới và diện tích cuối cùng đều là số nguyên. Số học số nguyên của Python cũng tránh được các vấn đề tràn có thể xuất hiện trong các ngôn ngữ có kiểu số nguyên có kích thước cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 5
2 -4 3
-5 2 6
-8 -1 4
```| Bánh quy | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | thông tin | -inf | thông tin | -inf | 
| (1,1,5) | -4 | 6 | -4 | 6 | 
| (2,-4,3) | -4 | 6 | -7 | 6 | 
| (-5,2,6) | -11 | 6 | -7 | 8 | 
| (-8,-1,4) | -12 | 6 | -7 | 8 | 

Chiều rộng cuối cùng là`18`và chiều cao cuối cùng là`15`, vùng sản xuất`270`. Dấu vết cho thấy mỗi cookie chỉ cần tác động đến bốn giá trị cực trị. 

### Ví dụ 2 

đầu vào:```
2
0 0 1
2 0 1
```| Bánh quy | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | thông tin | -inf | thông tin | -inf | 
| (0,0,1) | -1 | 1 | -1 | 1 | 
| (2,0,1) | -1 | 3 | -1 | 1 | 

Hình chữ nhật cuối cùng có chiều rộng`4`và chiều cao`2`, vậy câu trả lời là`8`. Ví dụ này xác nhận rằng các cookie chồng chéo hoặc chạm vào nhau không yêu cầu bất kỳ xử lý đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cookie được đọc và xử lý một lần. | 
| Không gian | O(1) | Chỉ có bốn giá trị biên được lưu trữ. | 

Kích thước đầu vào có thể đạt tới`100000`cookie và quá trình quét tuyến tính chỉ thực hiện một lượng công việc nhỏ không đổi trên mỗi cookie, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    
    solve()
    
    result = sys.stdout.getvalue()
    
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample 1
assert run("""4
1 1 5
2 -4 3
-5 2 6
-8 -1 4
""") == "270\n", "sample 1"

# minimum size
assert run("""1
0 0 1
""") == "4\n", "single cookie"

# all equal cookies
assert run("""3
5 5 2
5 5 2
5 5 2
""") == "16\n", "identical cookies"

# negative coordinates
assert run("""2
-10 -10 3
-5 -5 2
""") == "144\n", "negative coordinates"

# large values
assert run("""2
10000000 10000000 10000000
-10000000 -10000000 10000000
""") == "1600000000000000\n", "large boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một cookie có bán kính 1 | 4 | Tính toán đầu vào và đường kính tối thiểu | 
| Ba cookie giống hệt nhau | 16 | Các giá trị lặp lại không thay đổi ranh giới | 
| Cookie có tọa độ âm | 144 | Xử lý đúng tọa độ đã ký | 
| Tọa độ và bán kính rất lớn | 1600000000000000 | Số học diện tích lớn | 

## Vỏ cạnh 

Đối với một cookie, thuật toán khởi tạo hình chữ nhật chỉ từ cookie đó. Với đầu vào:```
1
0 0 5
```ranh giới trở thành`-5`,`5`,`-5`, Và`5`. Chiều rộng và chiều cao đều là`10`, vì vậy đầu ra là`100`. 

Đối với các cookie chạm hoặc chồng lên nhau, thuật toán không cố gắng hợp nhất các hình dạng. Nó chỉ quan tâm đến tọa độ chiếm đóng ngoài cùng. Với đầu vào:```
2
0 0 1
2 0 1
```phạm vi ngang là từ`-1`ĐẾN`3`và phạm vi dọc là từ`-1`ĐẾN`1`. Câu trả lời là`8`, điều này cho thấy sự chồng chéo không liên quan đến phép tính. 

Đối với các giá trị cực trị, thuật toán giữ các ranh giới số nguyên chính xác. Với đầu vào:```
2
10000000 10000000 10000000
-10000000 -10000000 10000000
```hình chữ nhật kéo dài từ`-20000000`ĐẾN`20000000`trên cả hai trục. Chiều rộng và chiều cao là mỗi`40000000`, cho diện tích là`1600000000000000`. Việc triển khai xử lý việc này mà không cần trường hợp đặc biệt vì số nguyên Python tăng lên khi cần thiết.
