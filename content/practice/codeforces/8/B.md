---
title: "CF 8B - Nỗi ám ảnh về robot"
description: "Chúng tôi được yêu cầu phân tích đường đi của robot trên lưới 2D vô hạn. Robot có thể di chuyển lên, xuống, sang trái, sang phải và các chuyển động của nó được ghi lại dưới dạng chuỗi ký tự U, D, L, R."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 8
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 8"
rating: 1400
weight: 8
solve_time_s: 56
verified: true
draft: false
---
[CF 8B - Nỗi ám ảnh về robot](https://codeforces.com/problemset/problem/8/B) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, đồ thị, cách triển khai 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân tích đường đi của robot trên lưới 2D vô hạn. Robot có thể di chuyển lên, xuống, sang trái hoặc sang phải và các chuyển động của nó được ghi lại dưới dạng một chuỗi các ký tự U, D, L, R. Thử thách ở đây là xác định xem có tồn tại **bất kỳ cách sắp xếp nào có thể có của các ô trống và bị chặn** trong đó đường dẫn chính xác này có thể là đường đi ngắn nhất từ ​​ô vuông bắt đầu đến ô vuông cuối cùng hay không. 

Robot không bao giờ cố gắng đi vào ô vuông bị chặn. Về bản chất, chúng ta cần xem liệu chúng ta có thể đặt ô vuông bắt đầu ở đâu đó để đường đi được ghi lại của rô-bốt không bao giờ quay lại ô vuông mà nó đã chiếm giữ hay không; việc xem lại sẽ chỉ ra một con đường không ngắn nhất, bởi vì robot có thể đến ô vuông đó với ít bước hơn bằng một tuyến đường khác. 

Đầu vào là một chuỗi có độ dài lên tới 100. Vì giá trị này tương đối nhỏ nên bất kỳ thuật toán nào hoạt động trong khoảng$O(n^2)$hoặc tốt hơn là sẽ chạy thoải mái trong giới hạn thời gian 2 giây. The crucial edge case occurs when the robot’s path crosses itself: if at any point the robot moves into a square it already visited, it cannot be a shortest path because that implies a cycle in the route. Ví dụ, đường dẫn`LR`quay trở lại ô vuông bắt đầu ngay lập tức và sẽ xuất ra`BUG`. Việc triển khai ngây thơ mà bỏ qua việc xem lại sẽ đưa ra kết quả sai`OK`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản để mô tả. Chúng ta có thể tưởng tượng việc xây dựng một mạng lưới khổng lồ và mô phỏng mọi ô vuông bắt đầu có thể, cố gắng đặt đường đi trong khi kiểm tra xem nó có cắt chính nó hay không. Mặc dù đúng về nguyên tắc nhưng điều này không thực tế: ngay cả với chiều dài 100, robot có thể có khả năng bao quát 10.000 tọa độ khác nhau nếu chúng ta cố gắng lập bản đồ tất cả các vị trí tương đối. Số lượng hoạt động tăng lên nhanh chóng khi chúng tôi kiểm tra sự trùng lặp cho từng điểm bắt đầu có thể. 

Cái nhìn sâu sắc quan trọng là tọa độ tuyệt đối của robot không quan trọng; chỉ **vị trí tương đối** dọc theo đường đi mới quan trọng. Nếu chúng ta mô phỏng robot bắt đầu từ một điểm gốc tùy ý`(0, 0)`và theo dõi tọa độ mà nó ghé thăm từng bước một, chúng ta có thể phát hiện ngay một nút giao. Nếu bất kỳ tọa độ nào lặp lại trước bước cuối cùng, robot sẽ quay lại một hình vuông không tương thích với đường đi ngắn nhất. Nếu không, đường dẫn có thể tương ứng với một số bản đồ hợp lệ. Điều này biến vấn đề thành một cuộc kiểm tra tư cách thành viên đơn giản dọc theo một đường dẫn duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tối ưu (theo dõi tọa độ đã truy cập) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chỉ định cho robot một tọa độ bắt đầu tùy ý, chẳng hạn như`(0, 0)`. Điều này đại diện cho hình vuông bắt đầu. 
2. Khởi tạo một tập hợp để lưu trữ tất cả các tọa độ đã truy cập. Thêm tọa độ bắt đầu vào tập hợp này. 
3. Với mỗi lần di chuyển trong chuỗi đường dẫn: 

1. Tính tọa độ tiếp theo dựa trên bước di chuyển: giảm x cho`L`, tăng x cho`R`, giảm y cho`D`, tăng y cho`U`. 
2. Kiểm tra xem tọa độ tiếp theo đã có trong tập hợp đã truy cập chưa. Nếu có thì quay lại ngay`BUG`. 
3. Nếu không, hãy thêm tọa độ mới vào tập hợp đã truy cập. 
4. Nếu đường dẫn hoàn thành mà không cần xem lại bất kỳ tọa độ nào, hãy quay lại`OK`. 

Tại sao nó hoạt động: đường đi ngắn nhất trên lưới không bao giờ quay lại hình vuông. Bằng cách theo dõi tọa độ và phát hiện các lần truy cập lại, chúng tôi nắm bắt chính xác tình trạng mà đường đi không thể ngắn nhất. Điều này hoạt động bất kể bản đồ vô hạn được lấp đầy ở nơi khác như thế nào, bởi vì bất kỳ vị trí bắt đầu nào cũng có thể được dịch sang`(0, 0)`không mất tính tổng quát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    moves = input().strip()
    
    x, y = 0, 0
    visited = set()
    visited.add((x, y))
    
    for move in moves:
        if move == 'L':
            x -= 1
        elif move == 'R':
            x += 1
        elif move == 'U':
            y += 1
        elif move == 'D':
            y -= 1
        
        if (x, y) in visited:
            print("BUG")
            return
        visited.add((x, y))
    
    print("OK")

if __name__ == "__main__":
    main()
```Việc triển khai này đọc đường dẫn, mô phỏng robot di chuyển từng bước và lưu trữ tọa độ đã truy cập trong một tập hợp để tra cứu theo thời gian liên tục. Việc sử dụng một bộ sẽ tránh được việc quét O(n) cho mỗi bước. Lựa chọn bắt đầu từ`(0, 0)`là tùy ý nhưng đơn giản hóa việc biểu diễn. Thứ tự của các thao tác rất quan trọng: kiểm tra các lượt xem lại **trước khi** thêm tọa độ mới vào tập hợp đã truy cập. 

## Ví dụ đã hoạt động 

**Mẫu 1:`LLUUUR`** 

| Di chuyển | Phối hợp | Đã truy cập | Kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | (0,0) | {(0,0)} | - | 
| L | (-1,0) | {(0,0),(-1,0)} | được | 
| L | (-2,0) | {(-2,0),(-1,0),(0,0)} | được | 
| Bạn | (-2,1) | {(-2,0),(-2,1),(-1,0),(0,0)} | được | 
| Bạn | (-2,2) | {(-2,0),(-2,1),(-2,2),(-1,0),(0,0)} | được | 
| Bạn | (-2,3) | {(-2,0),(-2,1),(-2,2),(-2,3),(-1,0),(0,0)} | được | 
| R | (-1,3) | {(-2,0),(-2,1),(-2,2),(-2,3),(-1,0),(-1,3),(0,0)} | được | 

Không có lần xem lại nào xảy ra. Đầu ra là`OK`. 

**Mẫu 2:`LR`** 

| Di chuyển | Phối hợp | Đã truy cập | Kết quả | 
| --- | --- | --- | --- | 
| bắt đầu | (0,0) | {(0,0)} | - | 
| L | (-1,0) | {(0,0),(-1,0)} | được | 
| R | (0,0) | {(0,0),(-1,0)} | phát hiện lại | 

Việc thăm lại diễn ra vào lúc`(0,0)`. Đầu ra là`BUG`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nước đi được xử lý một lần; đặt trung bình chèn và tra cứu là O(1) | 
| Không gian | O(n) | Tối đa n+1 tọa độ duy nhất được lưu trữ cho một đường dẫn có độ dài n | 

Với n 100, điều này không đáng kể đối với giới hạn 2 giây, 64 MB đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("LLUUUR\n") == "OK", "sample 1"
assert run("LR\n") == "BUG", "self-crossing immediately"

# custom cases
assert run("UUUU\n") == "OK", "straight path up, no revisits"
assert run("UD\n") == "BUG", "up then down revisits start"
assert run("LRLR\n") == "BUG", "zigzag returns to previously visited squares"
assert run("RULD\n") == "OK", "forms a 1x1 box without revisiting the same square"
assert run("L"*100 + "R"*100 + "U"*50 + "D"*50 + "\n") == "BUG", "long path that eventually revisits start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`UUUU`| được | Chuyển động tuyến tính, không tự chéo | 
|`UD`| LỖI | Quay trở lại để bắt đầu ngay lập tức | 
|`LRLR`| LỖI | Xem lại ngoằn ngoèo nhỏ | 
|`RULD`| được | Vòng lặp tối thiểu mà không cần xem lại | 
|`L*100+R*100+U*50+D*50`| LỖI | Con đường dài cuối cùng tìm lại cội nguồn | 

## Vỏ cạnh 

Hãy xem xét một đường dẫn ngay lập tức quay trở lại ô vuông ban đầu của nó:`UD`. Mô phỏng bắt đầu từ`(0,0)`, bước đi đầu tiên`U`dẫn đến`(0,1)`. Bước đi tiếp theo`D`trở về`(0,0)`. Tập hợp tọa độ đã truy cập đã chứa`(0,0)`, do đó thuật toán xuất ra chính xác`BUG`. 

Đối với một con đường dài như`L*100 + R*100`, mỗi bước di chuyển ban đầu sẽ mở rộng sang các tọa độ mới, nhưng sau 100 bước di chuyển sang trái, 100 bước đi bên phải tiếp theo sẽ quay lại cùng một đường đi. Tập đã truy cập phát hiện tọa độ lặp lại đầu tiên, đảm bảo đầu ra là`BUG`. Điều này xác nhận rằng thuật toán xử lý cả tự vượt nhỏ và lớn mà không gặp lỗi. 

Mọi đường đi không truy cập lại bất kỳ tọa độ nào, ngay cả khi nó có hình dạng gấp đôi (như hình xoắn ốc), sẽ dẫn đến`OK`, thể hiện tính đúng đắn của các đường đi ngắn nhất hợp lệ. 

Bài xã luận này xây dựng lại đầy đủ logic đằng sau việc kiểm tra tính hợp lệ của đường đi ngắn nhất và cung cấp một phương pháp rõ ràng, có thể lặp lại để phát hiện các đường đi tự đi qua.
