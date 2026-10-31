---
title: "CF 106369H - Cuộc đấu tay đôi của Smokin' Joe"
description: "Trò chơi bắt đầu bằng việc hoán vị các số từ 1 đến n. Hai người chơi liên tục chọn hai vị trí chưa đúng và hoán đổi giá trị ở các vị trí đó. Giá trị đạt đến vị trí của chính nó sẽ bị khóa và không thể tham gia vào các giao dịch hoán đổi sau này."
date: "2026-06-26T09:02:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106369
codeforces_index: "H"
codeforces_contest_name: "2023 UCF Local Programming Contest"
rating: 0
weight: 106369
solve_time_s: 50
verified: true
draft: false
---

[CF 106369H - Cuộc đấu tay đôi của Smokin' Joe](https://codeforces.com/problemset/problem/106369/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu bằng việc hoán vị các số từ`1`ĐẾN`n`. Hai người chơi liên tục chọn hai vị trí chưa đúng và hoán đổi giá trị ở các vị trí đó. Giá trị đạt đến vị trí của chính nó sẽ bị khóa và không thể tham gia vào các giao dịch hoán đổi sau này. Người chơi thực hiện hoán đổi cuối cùng thực hiện được toàn bộ hoán vị được sắp xếp sẽ thắng. Smokin Joe di chuyển đầu tiên. 

Đầu vào chứa kích thước của hoán vị và cách sắp xếp các giá trị hiện tại. Kết quả đầu ra hỏi người chơi nào có chiến lược chiến thắng giả sử cả hai người chơi đều chọn những nước đi giúp tối đa hóa cơ hội chiến thắng của họ. 

Hạn chế chính đó là`n`có thể đạt được`10^6`. Điều này ngay lập tức loại trừ các mô phỏng, lập trình động theo các trạng thái hoặc bất kỳ cách tiếp cận nào cố gắng khám phá các chuyển động có thể xảy ra. Số lượng hoán vị có thể có là rất lớn, do đó lời giải phải rút ra một thuộc tính toán học của hoán vị ban đầu và tính toán nó theo thời gian tuyến tính. 

Phần khó khăn là người chơi được phép thực hiện các giao dịch hoán đổi mà không cải thiện hoán vị ngay lập tức. Một giải pháp chỉ tính số lần hoán đổi tối thiểu cần thiết để sắp xếp là không đủ. Ví dụ, hoán vị```
3
2 3 1
```có thể được sắp xếp thành hai lần hoán đổi, vì vậy cách tiếp cận bất cẩn có thể khiến người chơi thứ hai thắng. Tuy nhiên, lý do thực sự sâu xa hơn: mọi trò chơi hợp lệ kết thúc ở hoán vị đã sắp xếp đều có cùng các nước đi chẵn lẻ. Số lần hoán đổi tối thiểu xảy ra trùng khớp với tính chẵn lẻ đó ở đây, nhưng bằng chứng phải xem xét tất cả các lựa chọn có thể xảy ra. 

Một trường hợp cạnh khác là hoán vị được tạo thành từ một số chu trình độc lập. Ví dụ:```
4
4 3 2 1
```Đầu ra đúng là:```
Oh No!
```Hoán vị là hai lần hoán đổi so với sắp xếp theo nghĩa thông thường. Vì tổng số chẵn lẻ của hoán vị là số chẵn nên số nước đi cần thiết để đạt đến trạng thái đã sắp xếp cũng phải là số chẵn, nghĩa là người chơi thứ hai sẽ có nước đi cuối cùng. 

Một lỗi phổ biến cuối cùng là xử lý các vị trí đã đúng không chính xác. Coi như:```
3
1 3 2
```Vị trí đầu tiên đã được cố định và không thể chạm vào được. Hai vị trí còn lại tạo thành một hoán đổi, vì vậy câu trả lời là:```
Smokin Joe!
```Việc triển khai tính tất cả các vị trí có sẵn sẽ không phản ánh các quy tắc thực tế. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô hình hóa cuộc đấu tay đôi. Từ hoán vị hiện tại, chúng tôi có thể tạo ra mọi hoán đổi có thể có giữa các vị trí có thể di chuyển, giải quyết đệ quy mọi trạng thái kết quả và đánh dấu các trạng thái mà người chơi hiện tại có thể buộc giành chiến thắng. Điều này đúng vì nó tuân theo các quy tắc trò chơi chính xác. 

Vấn đề là số lượng trạng thái. Hoán vị độ dài`n`có thể có`n!`các cách sắp xếp khác nhau và thậm chí một trạng thái có thể có nhiều giao dịch hoán đổi. Vì`n = 10^6`, thậm chí việc kiểm tra tất cả các giao dịch hoán đổi có thể xảy ra một lần cũng là điều không thể. 

Quan sát quan trọng xuất phát từ việc xem xét tính chẵn lẻ hoán vị. Mỗi lần hoán đổi sẽ thay đổi tính chẵn lẻ của một hoán vị. Hoán vị được sắp xếp là hoán vị đồng nhất, luôn có tính chẵn lẻ. Do đó, nếu hoán vị ban đầu có tính chẵn lẻ lẻ thì mọi đường dẫn đạt đến trạng thái được sắp xếp phải chứa một số bước di chuyển lẻ. Nếu nó có tính chẵn lẻ thì mỗi đường đi phải chứa một số bước di chuyển chẵn. 

Quy tắc khóa không thay đổi đối số này. Phần tử bị khóa chỉ đơn giản là một điểm cố định của hoán vị hiện tại. Mọi nước đi hợp pháp vẫn là sự hoán vị của hai vị trí đang di chuyển nên vẫn lật ngang. Vì trò chơi phải kết thúc ở hoán vị danh tính nên tính chẵn lẻ của số nước đi đã được xác định trước nước đi đầu tiên. 

Nếu số nước đi phải là số lẻ thì người chơi đầu tiên sẽ thực hiện nước đi cuối cùng. Nếu nó phải chẵn thì người chơi thứ hai sẽ làm như vậy. 

Tính chẵn lẻ của một hoán vị có thể được tìm thấy từ việc phân rã chu trình của nó. Một chu kỳ dài`k`đóng góp`k - 1`hoán đổi sang chẵn lẻ, do đó tổng số chẵn lẻ là:```
(n - number_of_cycles) mod 2
```Việc tìm kiếm các chu trình cần có thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt qua hoán vị và đếm xem nó chứa bao nhiêu chu kỳ. Bắt đầu từ vị trí chưa được truy cập và liên tục theo dõi giá trị được lưu ở đó cho đến khi quay lại vị trí đã truy cập. Đánh dấu mỗi vị trí đã ghé thăm một lần sẽ đưa ra tất cả các chu kỳ theo thời gian tuyến tính. 
2. Tính toán`n - cycles`. Mỗi chu kỳ có độ dài`k`yêu cầu`k - 1`chuyển vị để chia thành các điểm cố định, do đó, tổng các giá trị này qua tất cả các chu kỳ sẽ cho số mũ của dấu hoán vị. 
3. Kiểm tra tính chẵn lẻ của giá trị đó. Giá trị lẻ có nghĩa là hoán vị bắt đầu có tính chẵn lẻ lẻ, vì vậy mọi trò chơi hợp lệ đều có số lần hoán đổi lẻ và Smokin Joe thực hiện nước đi cuối cùng. 
4. Nếu giá trị là số chẵn, mọi trò chơi hợp lệ đều có số lần hoán đổi chẵn và Kẻ ngoài vòng pháp luật sẽ thực hiện nước đi cuối cùng. 

Tại sao nó hoạt động: 

Bất biến là tính chẵn lẻ của hoán vị. Hoán đổi luôn thay đổi tính chẵn lẻ này vì mọi chuyển vị đều đảo ngược dấu của hoán vị. Trạng thái cuối cùng duy nhất là hoán vị được sắp xếp, có tính chẵn lẻ. Do đó, tính chẵn lẻ của số lần hoán đổi được thực hiện trong bất kỳ trận đấu đã hoàn thành nào phải khớp chính xác với tính chẵn lẻ ban đầu của hoán vị. Vì người chơi chỉ quyết định chuỗi hợp lệ nào sẽ được chơi chứ không phải độ dài chẵn lẻ của chuỗi đó nên người chiến thắng được xác định hoàn toàn bằng hoán vị ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    visited = [False] * n
    cycles = 0

    for i in range(n):
        if not visited[i]:
            cycles += 1
            cur = i
            while not visited[cur]:
                visited[cur] = True
                cur = p[cur] - 1

    if (n - cycles) & 1:
        print("Smokin Joe!")
    else:
        print("Oh No!")

if __name__ == "__main__":
    solve()
```Mảng được lưu trữ bằng các chỉ số dựa trên 0, vì vậy giá trị`p[cur]`được chuyển đổi thành`p[cur] - 1`khi theo một chu kỳ. Số chu kỳ là thông tin duy nhất cần thiết, do đó không cần phải sửa đổi hoán vị hoặc mô phỏng hoán đổi. 

các`visited`mảng ngăn cản việc đi lại nhiều lần trong cùng một chu kỳ. Mỗi vị trí được nhập chính xác một lần, đó là lý do tại sao quá trình truyền tải vẫn tuyến tính ngay cả đối với kích thước đầu vào tối đa. 

Điều kiện cuối cùng sử dụng tính chẵn lẻ của`n - cycles`. Số lượng này có thể lớn nhưng số nguyên Python xử lý nó một cách an toàn. Chỉ có bit thấp nhất mới quan trọng, vì vậy hãy kiểm tra với`& 1`tránh những công việc không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 3 2
```Sự phân hủy chu trình là`(1)(2 3)`. 

| Bước | Chỉ số hiện tại | Hành động | Chu kỳ được tính | 
| --- | --- | --- | --- | 
| 1 | 1 | Chu trình phần tử đơn | 1 | 
| 2 | 2 | Theo đến vị trí 3 | 2 | 
| 3 | 3 | Đã truy cập | 2 | 

Việc tính toán là:```
n - cycles = 3 - 2 = 1
```Giá trị là số lẻ, do đó trò chơi có số nước đi là số lẻ. Smokin Joe thực hiện cuộc hoán đổi cuối cùng. 

Đầu ra:```
Smokin Joe!
```### Mẫu 2 

đầu vào:```
4
4 3 2 1
```Sự phân hủy chu trình là`(1 4)(2 3)`. 

| Bước | Chỉ số hiện tại | Hành động | Chu kỳ được tính | 
| --- | --- | --- | --- | 
| 1 | 1 | Theo dõi 1 -> 4 -> 1 | 1 | 
| 2 | 2 | Theo dõi 2 -> 3 -> 2 | 2 | 
| 3 | 3 | Đã truy cập | 2 | 
| 4 | 4 | Đã truy cập | 2 | 

Việc tính toán là:```
n - cycles = 4 - 2 = 2
```Giá trị là chẵn nên người chơi thứ hai thực hiện nước đi cuối cùng. 

Đầu ra:```
Oh No!
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được truy cập chính xác một lần trong khi tìm chu kỳ. | 
| Không gian | O(n) | Mảng đã truy cập lưu trữ một boolean cho mỗi vị trí. | 

Kích thước đầu vào có thể đạt tới một triệu phần tử, do đó cần có giải pháp tuyến tính. Thuật toán chỉ thực hiện một số thao tác cho mỗi phần tử và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    p = list(map(int, sys.stdin.readline().split()))

    visited = [False] * n
    cycles = 0

    for i in range(n):
        if not visited[i]:
            cycles += 1
            cur = i
            while not visited[cur]:
                visited[cur] = True
                cur = p[cur] - 1

    ans = "Smokin Joe!\n" if (n - cycles) & 1 else "Oh No!\n"

    sys.stdin = old_stdin
    return ans

assert run("""3
1 3 2
""") == "Smokin Joe!\n", "sample 1"

assert run("""4
4 3 2 1
""") == "Oh No!\n", "sample 2"

assert run("""2
2 1
""") == "Smokin Joe!\n", "single swap"

assert run("""5
1 2 3 5 4
""") == "Smokin Joe!\n", "fixed positions"

assert run("""6
2 1 4 3 6 5
""") == "Oh No!\n", "three independent swaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 2 1`|`Smokin Joe!`| Chu kỳ không cần thiết nhỏ nhất | 
|`5 / 1 2 3 5 4`|`Smokin Joe!`| Đã cố định vị trí | 
|`6 / 2 1 4 3 6 5`|`Oh No!`| Nhiều chu kỳ chẵn với tổng số chẵn lẻ | 

## Vỏ cạnh 

Đối với đầu vào```
3
1 3 2
```vị trí đầu tiên đã chính xác, vì vậy chu kỳ hoạt động duy nhất là hoán đổi giữa vị trí hai và ba. Thuật toán tìm tổng cộng hai chu kỳ, tính toán`3 - 2 = 1`và dự đoán một số nước đi lẻ. Người chơi đầu tiên thắng. 

Đối với đầu vào```
4
4 3 2 1
```hoán vị chứa hai chu kỳ riêng biệt có độ dài hai. Mỗi chu kỳ đóng góp một vào tính chẵn lẻ của trao đổi, tạo ra tổng cộng hai. Thuật toán tính giá trị chẵn, nghĩa là nước đi cuối cùng thuộc về người chơi thứ hai. 

Đối với một hoán vị gần như đã được sắp xếp với nhiều điểm cố định, chẳng hạn như```
5
1 2 3 5 4
```việc truyền tải theo chu kỳ một cách tự nhiên sẽ bỏ qua các vị trí cố định vì mỗi vị trí cố định chỉ đơn giản là một chu trình có độ dài bằng một. Chỉ có sự hoán đổi không cần thiết mới ảnh hưởng đến tính toán chẵn lẻ, do đó câu trả lời vẫn đúng.
