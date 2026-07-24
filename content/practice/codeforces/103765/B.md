---
title: "CF 103765B - \u5b57\u7b26\u4e32"
description: "Chúng ta được cho một trò chơi chơi trên một chuỗi các chữ cái viết thường. Hai người chơi luân phiên nhau, bắt đầu từ người chơi đầu tiên."
date: "2026-07-02T08:54:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103765
codeforces_index: "B"
codeforces_contest_name: "2022 Collegiate Programming Contest of Xiangtan University"
rating: 0
weight: 103765
solve_time_s: 47
verified: true
draft: false
---

[CF 103765B - \u5b57\u7b26\u4e32](https://codeforces.com/problemset/problem/103765/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một trò chơi chơi trên một chuỗi các chữ cái viết thường. Hai người chơi luân phiên nhau, bắt đầu từ người chơi đầu tiên. Một bước di chuyển bao gồm việc chọn bất kỳ cặp ký tự giống nhau liền kề nào trong chuỗi hiện tại và xóa cả hai ký tự đó, sau đó ghép các phần còn lại lại với nhau. Nếu người chơi không tìm được cặp như vậy trong lượt của mình thì người chơi đó sẽ thua ngay lập tức. 

Nhiệm vụ là xác định, đối với mỗi chuỗi bắt đầu nhất định, liệu người chơi đầu tiên có chiến lược chiến thắng hay không nếu cả hai người chơi đều chơi tối ưu. 

Các ràng buộc đủ chặt chẽ để tổng chiều dài của tất cả các trường hợp thử nghiệm có thể đạt tới một triệu. Điều đó loại trừ bất kỳ cách tiếp cận nào mô phỏng nhiều lần các bước di chuyển trên chuỗi theo cách đơn giản bằng cách quét tuyến tính trên mỗi lần di chuyển, vì trong trường hợp xấu nhất, mỗi lần xóa sẽ làm giảm chuỗi đi hai ký tự và có thể có O(n) các bước di chuyển như vậy, dẫn đến hành vi tổng thể là O(n²). 

Một điểm tinh tế là trò chơi không phải là việc xóa tùy ý mà chỉ là loại bỏ các cặp bằng nhau liền kề. Điều này làm cho cấu trúc có tính cục bộ cao nhưng không độc lập giữa các vị trí vì việc xóa có thể tạo ra các cặp liền kề mới không có mặt ban đầu. 

Các trường hợp cạnh quan trọng bao gồm các chuỗi không có cặp bằng nhau liền kề nào, các chuỗi trong đó tất cả các ký tự giống hệt nhau và các chuỗi trong đó việc xóa xếp tầng. Ví dụ, trong`"abccba"`, loại bỏ phần giữa`"cc"`tạo ra một cơ cấu cơ hội mới, nhưng không nhất thiết là chiến thắng dành cho người chơi đầu tiên vì tính chẵn lẻ của các nước đi sẵn có quan trọng hơn lòng tham của địa phương. 

Một trường hợp góc quan trọng khác là khi tồn tại nhiều cặp rời nhau. Trực giác ngây thơ có thể cho rằng mỗi cặp là một nước đi độc lập, nhưng điều đó không chính xác vì việc loại bỏ một cặp có thể phá hủy hoặc tạo ra các cặp khác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: mô phỏng trạng thái trò chơi. Đối với mỗi nước đi có thể xảy ra, hãy loại bỏ một cặp bằng nhau liền kề, đánh giá đệ quy chuỗi kết quả và quyết định xem người chơi hiện tại có thể giành chiến thắng hay không. Đây là một trò chơi DP cổ điển trên dây. 

Tuy nhiên, số lượng các trạng thái riêng biệt tăng lên một cách bùng nổ. Ngay cả khi chúng tôi ghi nhớ theo cấu hình chuỗi, mỗi lần xóa sẽ thay đổi tính liền kề trên toàn cầu và số lượng trạng thái có thể truy cập sẽ theo cấp số nhân trong trường hợp xấu nhất. Đối với một chuỗi như`"aaaaa...."`, mỗi lần di chuyển đều làm giảm độ dài nhưng tạo ra các cấu hình lân cận mới, vẫn mang lại cấu trúc phân nhánh khổng lồ. Điều này ngay lập tức vượt quá mọi giới hạn phức tạp khả thi. 

Điều quan trọng là mỗi lần di chuyển sẽ loại bỏ chính xác hai ký tự và không bao giờ thay đổi thứ tự tương đối của các ký tự còn lại. Quan trọng hơn, mỗi nước đi sẽ giảm độ dài đi hai, do đó trò chơi luôn diễn ra theo trình tự giảm dần các bước chẵn. Điều này có nghĩa là toàn bộ trò chơi thực sự được xác định bởi số lượng nước đi có thể thực hiện được, chứ không phải bởi nước đi nào được chọn. 

Nếu chúng ta nghĩ về quá trình xếp chồng, mỗi lần xóa tương ứng với việc lấy ra một cặp ký tự liền kề giống hệt nhau. Quá trình loại bỏ nhiều lần các cặp bằng nhau liền kề là xác định nếu chúng ta xử lý tham lam từ trái sang phải bằng cách sử dụng một ngăn xếp: bất cứ khi nào ký tự hiện tại bằng đỉnh của ngăn xếp, chúng ta sẽ loại bỏ phần trên cùng thay vì đẩy. 

Việc giảm tham lam này tính toán chuỗi rút gọn cuối cùng và đếm ngầm số lần xóa xảy ra. Mỗi lần xóa là một nước đi trong trò chơi. Vì mỗi nước đi giảm độ dài chính xác đi 2 nên tổng số nước đi là cố định bất kể thứ tự. Điều này khiến trò chơi trở thành một bài toán chẵn lẻ: nếu số lần xóa có thể là số lẻ thì người chơi đầu tiên sẽ thắng, nếu không thì người chơi thứ hai sẽ thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Trò chơi Brute Force DP | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm ngăn xếp + chẵn lẻ | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Khởi tạo một ngăn xếp trống biểu thị dạng rút gọn hiện tại của chuỗi. Ngăn xếp này mô hình hóa bất biến mà không có hai ký tự liền kề nào trong đó bằng nhau. 
2. Quét chuỗi từ trái qua phải theo từng ký tự. Đối với mỗi ký tự, so sánh nó với đầu ngăn xếp nếu ngăn xếp không trống. 
3. Nếu ngăn xếp không trống và phần tử trên cùng bằng ký tự hiện tại, hãy bật ngăn xếp thay vì đẩy. Điều này thể hiện việc thực hiện xóa một cặp bằng nhau liền kề. Lý do điều này đúng là vì bất kỳ cặp bằng nhau liền kề nào cuối cùng cũng phải bị xóa và việc trì hoãn việc xóa nó không ảnh hưởng đến tổng số lần xóa. 
4. Ngược lại, đẩy ký tự hiện tại vào ngăn xếp, mở rộng cấu trúc rút gọn hiện tại. 
5. Duy trì bộ đếm số lần xóa đã được thực hiện. Mỗi cửa sổ bật lên tương ứng với một cặp phù hợp sẽ tăng bộ đếm này lên một. 
6. Sau khi xử lý toàn bộ chuỗi, hãy kiểm tra tính chẵn lẻ của số lần xóa. Nếu là số lẻ, ghi "Có" vì người chơi đầu tiên thực hiện nước đi hợp lệ cuối cùng. Nếu chẵn thì xuất ra "No". 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý từng tiền tố của chuỗi bằng quy tắc ngăn xếp, ngăn xếp biểu thị dạng rút gọn hoàn toàn của tiền tố đó trong tất cả các chuỗi phát tối ưu có thể có và số lần loại bỏ được thực hiện là cố định bất kể thứ tự các nước đi hợp lệ. Điều này xuất phát từ thực tế là việc loại bỏ chỉ loại bỏ các cặp bằng nhau liền kề và không bao giờ tạo ra sự mơ hồ về tổng số: mỗi chuỗi trò chơi hợp lệ tương ứng với việc loại bỏ các cặp rời rạc theo một số thứ tự, nhưng tổng số cặp có thể loại bỏ là bất biến khi sắp xếp lại vì mỗi lần loại bỏ sẽ giảm độ dài đi hai và không ảnh hưởng đến tính chẵn lẻ của cấu trúc còn lại theo cách làm thay đổi tổng số lượng. 

Do đó, trò chơi giảm xuống việc đếm xem có bao nhiêu lần hủy như vậy xảy ra trong quá trình xếp chồng và quyết định người chiến thắng theo tính chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        s = input().strip()
        stack = []
        moves = 0
        
        for ch in s:
            if stack and stack[-1] == ch:
                stack.pop()
                moves += 1
            else:
                stack.append(ch)
        
        if moves % 2 == 1:
            out.append("Yes")
        else:
            out.append("No")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng ngăn xếp để mô phỏng việc hủy các ký tự bằng nhau liền kề. Mỗi lần hủy tương ứng chính xác với một nước đi hợp lệ trong trò chơi, do đó biến`moves`theo dõi tổng số bước di chuyển có sẵn khi chơi tối ưu. 

Chi tiết quan trọng là chúng ta không bao giờ cần phải xem xét lại các quyết định trước đó. Sau khi một cặp bị xóa, nó không thể ảnh hưởng đến việc xóa trong tương lai ngoại trừ việc hiển thị vùng lân cận mới và ngăn xếp sẽ xử lý việc đó một cách tự nhiên. 

Quyết định cuối cùng hoàn toàn dựa trên tính chẵn lẻ, đó là lý do tại sao không cần thêm lý luận về lý thuyết trò chơi. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"funny"`Chúng tôi mô phỏng ngăn xếp. 

| Bước | Nhân vật | Ngăn xếp | Di chuyển | 
| --- | --- | --- | --- | 
| 1 | f | f | 0 | 
| 2 | bạn | bạn ơi | 0 | 
| 3 | n | ôi trời | 0 | 
| 4 | n | bạn ơi | 1 | 
| 5 | y | ứ ứ | 1 | 

Nước đi cuối cùng = 1, là số lẻ nên kết quả là`"Yes"`. 

Điều này cho thấy một lần hủy đơn giản, xác nhận rằng thuật toán chỉ tính các lần xóa thực tế chứ không phải các lần xóa tiềm năng. 

### Ví dụ 2:`"abccba"`| Bước | Nhân vật | Ngăn xếp | Di chuyển | 
| --- | --- | --- | --- | 
| 1 | một | một | 0 | 
| 2 | b | a b | 0 | 
| 3 | c | a b c | 0 | 
| 4 | c | a b | 1 | 
| 5 | b | một | 2 | 
| 6 | một | trống | 3 | 

Nước đi cuối cùng = 3, lẻ, nên đáp án là`"Yes"`. 

Điều này thể hiện việc xóa theo tầng trong đó việc loại bỏ một cặp trung tâm sẽ làm lộ ra các cặp liền kề mới. Ngăn xếp nắm bắt phản ứng dây chuyền này một cách tự nhiên mà không cần quay lại rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nhân vật được đẩy và bật tối đa một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ các ký tự chưa từng có còn lại | 

Vì tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 10^6, giải pháp tuyến tính này dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(sys.stdin.readline())
    res = []
    for _ in range(T):
        s = sys.stdin.readline().strip()
        stack = []
        moves = 0
        for ch in s:
            if stack and stack[-1] == ch:
                stack.pop()
                moves += 1
            else:
                stack.append(ch)
        res.append("Yes" if moves % 2 == 1 else "No")
    return "\n".join(res)

# provided samples
assert run("3\nxtu\nfunny\nabcdcba\n") == "No\nYes\nNo"
assert run("1\nabccba\n") == "Yes"

# custom cases
assert run("1\na") == "No", "single char"
assert run("1\naa") == "Yes", "single move"
assert run("1\naabb") == "No", "two independent deletions"
assert run("1\naaa") == "Yes", "chain reaction collapse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`No`| không thể di chuyển được | 
|`"aa"`|`Yes`| di chuyển cưỡng bức duy nhất | 
|`"aabb"`|`No`| số lần xóa chẵn | 
|`"aaa"`|`Yes`| hủy xếp tầng | 

## Vỏ cạnh 

Đối với một nhân vật như`"a"`, ngăn xếp không bao giờ thực hiện pop, vì vậy`moves = 0`và đầu ra là`"No"`. Thuật toán xác định chính xác rằng không có nước đi hợp lệ nào tồn tại. 

Vì`"aaaa"`, quá trình này diễn ra tuần tự: hai ký tự đầu tiên hủy bỏ, sau đó hai ký tự còn lại hủy bỏ, tạo ra tổng cộng hai nước đi. Ngăn xếp ghi lại chính xác hai pop, do đó tính chẵn lẻ là chẵn và kết quả là`"No"`. 

Đối với các chuỗi xen kẽ như`"ababab"`, không có cặp bằng nhau liền kề nào được hình thành nên ngăn xếp chỉ tăng lên và không bao giờ xuất hiện. Số lần di chuyển vẫn bằng 0, tạo ra chính xác`"No"`vì không có động thái hợp pháp nào tồn tại ở bất kỳ giai đoạn nào.
