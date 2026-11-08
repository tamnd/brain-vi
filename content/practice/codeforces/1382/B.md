---
problem: 1382B
contest_id: 1382
problem_index: B
name: "Sequential Nim"
contest_name: "Codeforces Round 658 (Div. 2)"
rating: 1100
tags: ["dp", "games"]
answer: passed_samples
verified: true
solve_time_s: 283
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e6ef5-142c-83ec-96d7-74def36b8cb3
---

#CF 1382B - Nim tuần tự 

**Đánh giá:** 1100 
**Tags:** dp, trò chơi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4m 43s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e6ef5-142c-83ec-96d7-74def36b8cb3 

--- 

##Giải pháp 

## Hiểu vấn đề 

Trò chơi được chơi trên một hàng cọc. Trong mỗi lần di chuyển, chỉ có đống đá ngoài cùng bên trái còn chứa đá là quan trọng và người chơi có thể loại bỏ bất kỳ số lượng đá dương nào khỏi đống đó. Khi một cọc trống, việc chơi tiếp tục với cọc tiếp theo theo cách tương tự. Người chơi thay phiên nhau và người chơi không thể di chuyển vì tất cả cọc đều trống sẽ thua. 

Mặc dù điều này trông giống như nhiều cọc độc lập, nhưng hạn chế ngoài cùng bên trái biến nó thành một chuỗi phát triển duy nhất trong đó các cọc lần lượt cạn kiệt và mỗi lần cạn kiệt có thể "lật đổ" tình huống chiến lược tùy thuộc vào số lần di chuyển đã được thực hiện bên trong cọc đó. 

Đầu vào bao gồm một số trò chơi độc lập. Đối với mỗi trò chơi, chúng ta phải quyết định xem người chơi đầu tiên có bị buộc phải thắng trong lối chơi tối ưu hay không. 

Các ràng buộc cho phép tổng cộng lên tới một trăm nghìn cọc trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng xử lý từng viên đá hoặc thậm chí từng chuyển động riêng lẻ. Bất kỳ giải pháp nào cũng phải quét từng trường hợp thử nghiệm trong thời gian tuyến tính, vì lý luận bậc hai hoặc mỗi bước di chuyển sẽ vượt quá giới hạn theo một số bậc độ lớn. 

Trường hợp khó phát hiện khi tất cả các cọc đều chứa đúng một viên đá. Trong tình huống đó, mỗi nước đi sẽ làm trống hoàn toàn một cọc, vì vậy trò chơi chỉ còn là một sự luân phiên đơn giản giữa các cọc. Ví dụ, đối với`[1, 1, 1]`, người chơi đầu tiên thắng, trong khi đối với`[1, 1, 1, 1]`, người chơi thứ hai thắng. Một trực giác ngây thơ có thể tập trung không chính xác vào tổng số quân chẵn lẻ, nhưng đó không phải là điều chi phối trò chơi. 

Một trường hợp cạnh quan trọng khác xuất hiện khi một đống lớn hơn một tồn tại sớm trong chuỗi. Ví dụ, trong`[2, 5, 4]`, người chơi đầu tiên sẽ thắng ngay lập tức bằng cách kiểm soát cách giảm cọc đầu tiên. Tuy nhiên, nếu đống lớn đầu tiên đó xuất hiện muộn hơn, chẳng hạn như`[1, 1, 2, ...]`, tính chẵn lẻ thay đổi tùy thuộc vào vị trí của nó chứ không phải chỉ giá trị của nó. 

Hai hành vi này, “tất cả những cái” và “đống không phải một đầu tiên”, là những trường hợp cấu trúc duy nhất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng bước di chuyển của trò chơi. Chúng tôi sẽ duy trì chỉ số cọc hiện tại và liên tục trừ đi nó, luân phiên người chơi cho đến khi tất cả các cọc trống. Trong trường hợp xấu nhất, một đống có thể chứa tới một tỷ viên đá, nghĩa là một trường hợp thử nghiệm có thể cần tới$10^9$hoạt động vượt quá giới hạn cho phép. Ngay cả khi chúng tôi chỉ mô phỏng chuyển tiếp cọc, chúng tôi vẫn có nguy cơ làm việc tuyến tính trên mỗi cọc trong mỗi lần di chuyển, điều này là không cần thiết. 

Quan sát quan trọng là trong một đống, số lượng đá chính xác không quan trọng bằng việc nó bằng một hay lớn hơn một. Nếu một cọc có kích thước bằng một thì nó bị ép buộc: người chơi trên đó sẽ loại bỏ nó và ngay lập tức chuyển quyền kiểm soát cho cọc tiếp theo. Nếu một cọc có kích thước lớn hơn một, thì khi người chơi tiếp cận lần đầu tiên, người chơi đó có thể kiểm soát số lần di chuyển nội bộ xảy ra trước khi nó trở thành kích thước một, quyết định một cách hiệu quả liệu “chẵn lẻ lượt” có lật ở ranh giới đó hay không. 

Điều này làm cho toàn bộ trò chơi chỉ còn việc xác định cọc đầu tiên không bằng một. Nếu tất cả các cọc là một, kết quả chỉ phụ thuộc vào số lượng nước đi cưỡng bức như vậy tồn tại. Nếu không, vị trí của cọc không phải một đầu tiên sẽ xác định xem người chơi đầu tiên có thể duy trì quyền kiểm soát hay mất nó. 

Do đó, chúng tôi giảm vấn đề xuống còn một lần quét tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng số đá) | O(1) | Quá chậm | 
| Giảm quét tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét các cọc từ trái sang phải đồng thời kiểm tra xem mỗi cọc có đúng một viên đá hay không. Điều này xác định liệu chúng ta có còn ở trong phân đoạn buộc phải di chuyển mà không có quyết định nào tồn tại hay không. 
2. Dừng lại ở đống đầu tiên có kích thước lớn hơn một. Vị trí này rất quan trọng vì đây là điểm đầu tiên mà người chơi giành quyền kiểm soát thời gian trò chơi tồn tại trong một cọc. 
3. Nếu có cọc như vậy ở chỉ số i thì xác định người thắng dựa vào i chẵn hay lẻ. Nếu tôi là số lẻ, người chơi đầu tiên sẽ thắng vì họ đạt được cọc có thể kiểm soát đầu tiên theo lượt chẵn lẻ thuận lợi. Nếu tôi chẵn thì người chơi thứ hai sẽ thừa hưởng lợi thế đó. 
4. Nếu không có cọc nào như vậy, nghĩa là mỗi cọc có đúng một viên đá, hãy tính người thắng cuộc dựa trên n. Nếu n lẻ, người chơi đầu tiên sẽ thắng vì họ là người thực hiện nước đi cuối cùng. Nếu n chẵn thì người thứ hai đi nước cuối cùng và thắng. 

Lý do chỉ tính chẵn lẻ là đủ vì mỗi cọc có kích thước một sẽ tiêu tốn đúng một nước đi và lật đổ người chơi. Chỉ cọc đầu tiên có kích thước lớn hơn một mới có thể phá vỡ sự luân phiên bắt buộc này bằng cách đưa ra một phân đoạn có thể kiểm soát được và vị trí của nó sẽ xác định xem lợi thế ngang bằng vẫn thuộc về người chơi đầu tiên hay thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        idx = -1
        for i, v in enumerate(a):
            if v > 1:
                idx = i
                break
        
        if idx == -1:
            out.append("First" if n % 2 == 1 else "Second")
        else:
            out.append("First" if idx % 2 == 0 else "Second")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử và thực hiện một lần chuyển qua mảng. Vòng lặp tìm kiếm đống đầu tiên không bằng một, lưu trữ chỉ mục của nó. Nếu không có thì quyết định chỉ phụ thuộc vào việc số lượng cọc có phải là số lẻ hay không. Ngược lại, tính chẵn lẻ của chỉ số sẽ xác định người chiến thắng. 

Chi tiết triển khai chính là lập chỉ mục dựa trên số không. Lý do sử dụng các vị trí dựa trên 1 về mặt khái niệm, do đó mã áp dụng trực tiếp các kiểm tra chẵn hoặc lẻ trên chỉ mục dựa trên 0 mà không cần điều chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 5, 4]`| Bước | Chỉ mục | Giá trị | Tìm thấy đầu tiên không phải 1 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | vâng | dừng lại | 

Cọc đầu tiên đã chứa nhiều hơn một viên đá nên người chơi đầu tiên ngay lập tức kiểm soát điểm quyết định quan trọng. Vì điều này xảy ra ở chỉ số 0 nên người chơi đầu tiên sẽ thắng. 

Điều này chứng tỏ rằng một đống lớn sớm đảm bảo quyền kiểm soát chiến lược ngay lập tức. 

### Ví dụ 2 

đầu vào:`[1, 2, 3, 4, 5, 6]`| Bước | Chỉ mục | Giá trị | Tìm thấy đầu tiên không phải 1 | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | không | tiếp tục | 
| 2 | 1 | 2 | vâng | dừng lại | 

Cọc không tầm thường đầu tiên xuất hiện ở chỉ số 1. Điều đó có nghĩa là người chơi thứ hai đạt đến điểm quyết định có ý nghĩa đầu tiên, chuyển quyền kiểm soát khỏi người chơi đầu tiên. Kết quả là người chơi thứ hai giành chiến thắng. 

Điều này cho thấy một khối dẫn đầu duy nhất làm trì hoãn việc chuyển giao quyền kiểm soát và lật ngược kết quả như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cọc được kiểm tra một lần cho đến khi tìm thấy cọc đầu tiên | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Tổng công việc trong tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số cọc, phù hợp thoải mái trong các ràng buộc của$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        idx = -1
        for i, v in enumerate(a):
            if v > 1:
                idx = i
                break
        if idx == -1:
            res.append("First" if n % 2 == 1 else "Second")
        else:
            res.append("First" if idx % 2 == 0 else "Second")
    return "\n".join(res)

# provided samples
assert run("""7
3
2 5 4
8
1 1 1 1 1 1 1 1
6
1 2 3 4 5 6
6
1 1 2 1 2 2
1
1000000000
5
1 2 2 1 1
3
1 1 1
""") == """First
Second
Second
First
First
Second
First"""

# all ones edge case
assert run("""1
4
1 1 1 1
""") == "Second"

# single pile large
assert run("""1
1
1000000000
""") == "First"

# first element large
assert run("""1
3
5 1 1
""") == "First"

# delayed large pile
assert run("""1
5
1 1 1 2 1
""") == "Second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | Thứ hai | quy tắc chẵn lẻ khi không tồn tại phân nhánh | 
| cọc đơn lớn | Đầu tiên | trường hợp tối thiểu | 
| cọc lớn | Đầu tiên | kiểm soát ngay lập tức | 
| cọc không một | Thứ hai | hiệu ứng chẵn lẻ chỉ số | 

## Vỏ cạnh 

Khi tất cả các cọc chứa chính xác một viên đá, trò chơi sẽ trở thành một sự luân phiên nghiêm ngặt của các nước đi trên các cọc. Đối với đầu vào`[1, 1, 1, 1]`, thuật toán phát hiện không có cọc nào lớn hơn một, do đó, nó quay lại kiểm tra tính chẵn lẻ của 4 và trả về chính xác Thứ hai. Trình tự các nước đi bắt buộc đảm bảo người chơi thứ hai thực hiện nước đi cuối cùng. 

Khi bản thân đống đầu tiên lớn hơn một, chẳng hạn như`[7, 1, 1]`, quá trình quét dừng ở chỉ số 0. Vì chỉ số 0 là số chẵn nên thuật toán trả về Đầu tiên. Trong trường hợp này, người chơi đầu tiên không bao giờ mất quyền kiểm soát tính chẵn lẻ vì không có chuỗi đá đơn bắt buộc nào trước điểm quyết định. 

Khi một đống không phải một xuất hiện sau đó, chẳng hạn như`[1, 1, 1, 5, 1]`, quá trình quét dừng ở chỉ số 3. Thuật toán trả về Đầu tiên vì 3 là số lẻ trong lập chỉ mục dựa trên 0, nghĩa là người chơi thứ hai đạt đến cọc có thể kiểm soát đầu tiên và tính chẵn lẻ sẽ thay đổi theo chúng.
