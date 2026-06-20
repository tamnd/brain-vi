---
title: "CF 1020B - Huy Hiệu"
description: "Chúng ta được cung cấp một cấu trúc có định hướng cho các học sinh trong đó mỗi học sinh chỉ vào chính xác một học sinh khác. Điều này tạo thành một đồ thị hàm số: mỗi nút đều có một mức độ ngoài cùng, do đó việc bắt đầu từ bất kỳ nút nào và lặp đi lặp lại các con trỏ cuối cùng buộc chúng ta phải đi vào một chu trình."
date: "2026-06-16T22:00:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1020
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 503 (by SIS, Div. 2)"
rating: 1000
weight: 1020
solve_time_s: 90
verified: true
draft: false
---

[CF 1020B - Huy hiệu](https://codeforces.com/problemset/problem/1020/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, dfs và tương tự, đồ thị 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc có định hướng cho các học sinh trong đó mỗi học sinh chỉ vào chính xác một học sinh khác. Điều này tạo thành một đồ thị hàm số: mỗi nút đều có một mức độ ngoài cùng, do đó việc bắt đầu từ bất kỳ nút nào và lặp đi lặp lại các con trỏ cuối cùng buộc chúng ta phải đi vào một chu trình. 

Một giáo viên bắt đầu với một học sinh nào đó`a`. Học sinh đó ngay lập tức được chấm điểm một lần. Sau đó giáo viên tiếp tục di chuyển từ học sinh hiện tại`i`ĐẾN`p[i]`, đánh dấu từng học sinh đã đến thăm. Quá trình này dừng lại khi giáo viên đến gặp một học sinh đã được chấm điểm và học sinh đó nhận được điểm thứ hai. 

Nhiệm vụ là xác định, đối với mọi nút bắt đầu có thể`a`, học sinh nào sẽ là người đầu tiên được viếng thăm hai lần. 

Ràng buộc`n ≤ 1000`đủ nhỏ để mô phỏng O(n2) hoặc thậm chí O(n³) là an toàn. Bất kỳ giải pháp nào thực hiện một lần truyền tải mới từ mỗi nút bắt đầu đều đã nằm trong giới hạn, vì mỗi lần truyền tải có thể truy cập nhiều nhất`n`các nút trước khi lặp lại. 

Một điểm tinh tế là quá trình này không chỉ đơn giản là “tìm một mục nhập chu kỳ” theo nghĩa tĩnh, bởi vì tập hợp các nút được truy cập phụ thuộc vào điểm bắt đầu. Nút lặp lại đầu tiên là nút đầu tiên xuất hiện hai lần theo thứ tự truyền tải, không nhất thiết là điểm bắt đầu của một chu trình theo nghĩa phân rã thông thường. 

Các trường hợp cạnh quan trọng bao gồm: 

Một vòng lặp tự như`p[i] = i`. Bắt đầu từ một nút như vậy sẽ ngay lập tức truy cập lại nó ở lần di chuyển đầu tiên, vì vậy câu trả lời chỉ là tầm thường. 

Một trường hợp khác là khi tất cả các nút cuối cùng đều chảy vào một chu kỳ duy nhất. Ngay cả khi đó, các điểm bắt đầu khác nhau trong một phần đuôi có thể dẫn đến các nút lặp lại đầu tiên khác nhau vì điểm vào trong chu trình phụ thuộc vào cách đi qua đường dẫn. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: mô phỏng chuyển động của giáo viên cho từng học sinh mới bắt đầu. Chúng tôi duy trì một mảng đã truy cập, đi dọc theo`p[current]`và dừng lại ngay khi chúng tôi đến một nút đã thấy trong mô phỏng đó. Nút đó được ghi lại làm câu trả lời cho sự bắt đầu. 

Điều này hoạt động vì mỗi mô phỏng sẽ tái tạo lại quy trình chính xác được mô tả trong câu lệnh một cách độc lập. Sự đúng đắn là ngay lập tức vì chúng tôi thực sự tuân theo các quy tắc từng bước một. 

Sự kém hiệu quả xuất phát từ việc lặp lại các bước đi giống nhau từ đầu. Mỗi mô phỏng có thể đi qua một chuỗi dài có độ dài O(n) và chúng tôi thực hiện điều này cho tất cả n điểm bắt đầu, đưa ra các bước tổng thể là O(n2). Với n lên tới 1000, điều này vẫn ổn, nhưng nó gợi ý về một cấu trúc sâu hơn: mọi quá trình truyền tải đều mang tính xác định và chỉ phụ thuộc vào các con trỏ sau trong biểu đồ hàm. 

Quan sát quan trọng là câu trả lời cho nút bắt đầu chỉ phụ thuộc vào vị trí đường đi của nó giao nhau lần đầu tiên trong một chu trình hoặc quay lại nút đã có trên đường đi của nó. Vì đồ thị là hàm số nên mọi nút đều thuộc về một đuôi dẫn vào một chu trình hoặc thuộc về chính chu trình đó. Nút lặp lại đầu tiên cho bất kỳ lần bắt đầu nào chính xác là nút đầu tiên trên đường dẫn mà lượt truy cập tiếp theo sẽ vi phạm thuộc tính “lần đầu tiên nhìn thấy” trong quá trình truyền tải đó. 

Vì n nhỏ nên chúng ta không cần tiền xử lý nặng hoặc phân rã chu trình; mô phỏng trực tiếp với một mảng mới được truy cập là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi học sinh bắt đầu`a`, tạo một mảng boolean mới`visited`kích thước`n + 1`. Điều này đảm bảo mỗi mô phỏng là độc lập và không bị rò rỉ trạng thái khi bắt đầu. 
2. Khởi tạo`current = a`. 
3. Liên tục kiểm tra xem`current`đã được viếng thăm. Nếu đúng như vậy thì điều này có nghĩa là chúng ta đã quay lại nút đã thấy trong mô phỏng này, vì vậy`current`là câu trả lời cho điểm khởi đầu này. 
4. Nếu nó chưa được truy cập, hãy đánh dấu`current`như đã ghé thăm. 
5. Di chuyển`current = p[current]`và lặp lại. 
6. Lưu nút đầu tiên đã kích hoạt điều kiện “đã truy cập” làm câu trả lời cho`a`. 

Lý do thứ tự bước này quan trọng là việc kiểm tra phải diễn ra trước khi tiếp tục, vì nút lặp lại được xác định là nút chúng ta vừa nhập chứ không phải nút chúng ta sắp rời khỏi. 

### Tại sao nó hoạt động 

Trong quá trình mô phỏng từ một điểm khởi đầu cố định`a`, thuật toán duy trì bất biến rằng`visited[x]`đúng khi và chỉ nếu nút`x`đã được truy cập trước đó trong lần duyệt chính xác này. Vì biểu đồ có bậc ngoài 1 nên chuỗi các nút được truy cập tạo thành một bước đi duy nhất. Lần đầu tiên chúng ta gặp một nút đã được đánh dấu, nút đó phải là nút lặp lại sớm nhất trong chuỗi theo định nghĩa về cách chúng tôi duy trì`visited`. Không có nút nào trước đó có thể lặp lại, nếu không vòng lặp sẽ kết thúc sớm hơn. Điều này đảm bảo nút được trả về chính xác là nút đầu tiên được truy cập hai lần trong quy trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))
    
    ans = [0] * (n + 1)
    
    for start in range(1, n + 1):
        visited = [False] * (n + 1)
        cur = start
        
        while True:
            if visited[cur]:
                ans[start] = cur
                break
            visited[cur] = True
            cur = p[cur]
    
    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo quá trình truyền tải chính xác như được mô tả. Chi tiết quan trọng là chúng tôi kiểm tra`visited[cur]`trước khi đánh dấu hoặc di chuyển, vì sự lặp lại được phát hiện tại thời điểm chúng ta hạ cánh trên một nút đã được nhìn thấy. Mỗi lần khởi động lại tính toán lại mảng đã truy cập của chính nó để tránh nhiễu giữa các mô phỏng. 

Mảng 1 chỉ mục giúp đơn giản hóa việc ánh xạ trực tiếp từ nhãn sinh viên sang chỉ mục, tránh lỗi sai từng cái một khi đọc`p`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 2
```Chúng tôi tính toán riêng từng lần bắt đầu. 

Vì`start = 1`, chúng tôi theo dõi quá trình truyền tải: 

| Bước | Hiện tại | Đã ghé thăm trước đây | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | {} | đánh dấu 1 | 
| 2 | 2 | {1} | đánh dấu 2 | 
| 3 | 3 | {1,2} | đánh dấu 3 | 
| 4 | 2 | {1,2,3} | tìm thấy lặp lại | 

Câu trả lời là 2. 

cho`start = 2`: 

| Bước | Hiện tại | Đã ghé thăm trước đây | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | {} | đánh dấu 2 | 
| 2 | 3 | {2} | đánh dấu 3 | 
| 3 | 2 | {2,3} | tìm thấy lặp lại | 

Câu trả lời là 2. 

cho`start = 3`: 

| Bước | Hiện tại | Đã ghé thăm trước đây | Hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | {} | đánh dấu 3 | 
| 2 | 2 | {3} | đánh dấu 2 | 
| 3 | 3 | {2,3} | tìm thấy lặp lại | 

Câu trả lời là 3. 

Điều này xác nhận rằng lần lặp lại đầu tiên phụ thuộc vào vị trí truyền tải đi vào chu trình chứ không chỉ chính chu kỳ đó. 

### Ví dụ 2 

Hãy xem xét:```
4
1 2 3 4
```Mỗi nút trỏ tới nút tiếp theo, tạo thành một chu trình. 

Vì`start = 1`, chúng tôi ghé thăm`1 → 1`, vậy đáp án là 1 

cho`start = 2`, chúng tôi ghé thăm`2 → 2`, vậy đáp án là 2 

Tương tự cho những người khác. Mỗi nút là một nút tự quay lại theo nghĩa truyền tải này bởi vì chúng tôi ngay lập tức truy cập lại sau khi duyệt qua tất cả các nút đã thấy trong lần chạy đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi lần trong số n lần khởi động thực hiện quá trình truyền tải tối đa n bước | 
| Không gian | O(n) | Mảng đã truy cập trên mỗi lần truyền tải | 

Với`n ≤ 1000`, tối đa khoảng 1e6 thao tác xảy ra, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""3
2 3 2
""") == "2 2 3"

# self-loop case
assert run("""1
1
""") == "1"

# simple chain
assert run("""4
2 3 4 4
""") == "4 4 4 4"

# all point to 1
assert run("""4
1 1 1 1
""") == "1 1 1 1"

# cycle
assert run("""3
2 3 1
""") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tự lặp nút đơn | 
| chuỗi theo chu kỳ | 4 4 4 4 | đuôi dẫn vào chu kỳ | 
| tất cả đến 1 | 1 1 1 1 | hội tụ hình ngôi sao | 
| 2 3 1 | 1 2 3 | hành vi chu trình thuần túy | 

## Vỏ cạnh 

Một vòng lặp tự như`p[i] = i`là trường hợp trực tiếp nhất. Bắt đầu lúc`i`, chúng tôi đánh dấu nó và ngay lập tức nhìn thấy nó sau một lần chuyển đổi, do đó thuật toán trả về`i`chính xác bởi vì`visited[i]`đã đúng khi chúng ta xem lại nó. 

Một chuỗi dẫn vào một chu kỳ hoạt động tương tự nhưng lặp lại chậm hơn. Ví dụ`1 → 2 → 3 → 3`. Bắt đầu từ 1, chúng ta truy cập 1, 2, 3, sau đó cố gắng chuyển sang 3 lần nữa. Vì 3 đã được đánh dấu nên nó được xác định chính xác là nút lặp lại đầu tiên. Mô phỏng đảm bảo mục nhập chu trình được phát hiện chính xác ở lần xem lại đầu tiên, không sớm hơn hoặc muộn hơn.
