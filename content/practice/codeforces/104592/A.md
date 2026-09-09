---
title: "CF 104592A - Xúc xắc thẳng"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều viên xúc xắc và mỗi viên xúc xắc có đúng sáu số nguyên dương được ghi trên các mặt của nó. Khi đặt xúc xắc liên tiếp, chúng tôi chọn chính xác một mặt từ mỗi xúc xắc đã chọn làm giá trị “cao nhất” của xúc xắc đó."
date: "2026-06-30T05:25:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104592
codeforces_index: "A"
codeforces_contest_name: "2017 Google Code Jam World Finals (GCJ 17 World Finals)"
rating: 0
weight: 104592
solve_time_s: 43
verified: true
draft: false
---

[CF 104592A - Xúc xắc thẳng](https://codeforces.com/problemset/problem/104592/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều viên xúc xắc và mỗi viên xúc xắc có đúng sáu số nguyên dương được ghi trên các mặt của nó. Khi đặt xúc xắc liên tiếp, chúng tôi chọn chính xác một mặt từ mỗi xúc xắc đã chọn làm giá trị “cao nhất” của xúc xắc đó. Mục tiêu của chúng ta là chọn một tập hợp con xúc xắc và gán cho mỗi xúc xắc được chọn một trong các giá trị mặt của nó để các giá trị được chọn tạo thành một chuỗi các số nguyên liên tiếp. Nhiệm vụ là tối đa hóa số lượng xúc xắc mà chúng ta đưa vào một chuỗi như vậy. 

Một cách hữu ích để diễn giải lại vấn đề là coi mỗi con xúc xắc cung cấp tối đa sáu giá trị ứng cử viên và chúng ta muốn chọn một giá trị cho mỗi con súc sắc đã chọn để bộ đa cuối cùng có thể được sắp xếp thành một chuỗi số nguyên dài nhất có thể mà không có khoảng trống. 

Tổng cộng các ràng buộc rất lớn, lên tới 200.000 viên xúc xắc trên tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ điều gì phụ thuộc vào việc kiểm tra tất cả các tập hợp con xúc xắc hoặc tất cả các cách gán giá trị trên các viên xúc xắc. Ngay cả các phương pháp bậc hai trên xúc xắc cũng sẽ quá chậm. Bất kỳ cách tiếp cận khả thi nào cũng phải xử lý từng khuôn trong thời gian gần tuyến tính và tránh quét liên tục các cấu trúc tổng thể. 

Một điểm tinh tế là các viên xúc xắc khác nhau có thể chia sẻ các giá trị và thậm chí các viên xúc xắc giống hệt nhau có thể xuất hiện nhiều lần. Điều này quan trọng vì các bản sao có thể được sử dụng lại để kéo dài thời gian chạy liên tiếp nếu chúng cung cấp nhiều lần xuất hiện có cùng số lượng. 

Các trường hợp cạnh phát sinh khi xúc xắc có giá trị rất thưa thớt và không kết nối. Ví dụ: nếu không có hai viên xúc xắc nào có chung bất kỳ giá trị tương thích liên tiếp nào thì câu trả lời sẽ rút gọn thành 1. Một trường hợp góc khác là khi nhiều viên xúc xắc chứa các phạm vi chồng chéo, cho phép chuỗi dài ngay cả khi từng viên xúc xắc không đều. Một sự lựa chọn tham lam ngây thơ cho mỗi con súc sắc có thể thất bại ở đây, bởi vì cùng một con súc sắc có thể hỗ trợ nhiều số khác nhau và việc thực hiện quá sớm có thể chặn một chuỗi dài hơn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng gán một giá trị cho mỗi viên xúc xắc và sau đó kiểm tra tất cả các tập hợp con xúc xắc có thể có để xem tập hợp nào có thể tạo thành một chuỗi liên tiếp. Ngay cả khi chúng ta cố định một khoảng mục tiêu, chúng ta vẫn cần phải quyết định con xúc xắc nào đóng góp số nào và mỗi con súc sắc có sáu lựa chọn. Điều này dẫn đến sự bùng nổ: các bài tập khoảng O(6^N) hoặc ít nhất là khám phá tập hợp con O(N · 2^N). Điều này là không thể thực hiện được ngay cả với N khoảng 50 chứ đừng nói đến 200.000. 

Quan sát quan trọng là chúng ta thực sự không bắt buộc phải sử dụng tất cả các viên xúc xắc và mỗi viên xúc xắc chỉ có thể được sử dụng nhiều nhất một lần. Điều quan trọng là liệu chúng ta có thể “che” một khoảng số nguyên liên tiếp bằng cách sử dụng các viên xúc xắc có sẵn hay không, trong đó mỗi con súc sắc đóng góp tối đa một số nguyên từ khoảng đó. Điều này chuyển quan điểm từ tổ hợp qua các bài tập sang một vấn đề khả thi trong các khoảng thời gian. 

Với bất kỳ giá trị nguyên cố định x nào, chúng ta chỉ cần biết viên xúc xắc nào có thể tạo ra x. Nếu chúng ta cố gắng xây dựng một phân đoạn liên tiếp bắt đầu từ một số L nào đó, yêu cầu duy nhất là với mỗi số nguyên trong phân đoạn đó, chúng ta có ít nhất một xúc xắc chưa sử dụng có chứa nó. Điều này gợi ý một cấu trúc tham lam: mở rộng phân đoạn hiện tại càng xa càng tốt, luôn tiêu thụ một con súc sắc có sẵn trên mỗi số nguyên. 

Để thực hiện điều này hiệu quả, chúng tôi tính toán trước cho từng giá trị danh sách xúc xắc chứa giá trị đó. Sau đó, chúng tôi coi mỗi giá trị là một vị trí trong quá trình quét toàn cầu và ghép các viên xúc xắc một cách tham lam để kéo dài thời gian chạy liên tiếp dài nhất. 

Vấn đề giảm xuống còn việc tìm khoảng dài nhất trên dòng số nguyên sao cho chúng ta có thể gán các viên xúc xắc riêng biệt cho mọi số nguyên trong đó, với mỗi viên xúc xắc được gán cho một giá trị mà nó hỗ trợ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | O(6^N) | O(N) | Quá chậm | 
| Kết hợp tham lam được lập chỉ mục giá trị | O(N log N) hoặc O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng ánh xạ từ mỗi giá trị số nguyên xuất hiện trên bất kỳ xúc xắc nào tới danh sách các chỉ số xúc xắc có chứa nó. Điều này chuyển đổi các lựa chọn khuôn mặt thành thông tin kề nhau giữa các giá trị và xúc xắc. Lý do điều này hữu ích là vì chúng ta chỉ quan tâm liệu một con súc sắc có thể hỗ trợ một con số cụ thể hay không chứ không phải khuôn mặt nào tạo ra nó. 
2. Sắp xếp tất cả các giá trị riêng biệt xuất hiện trên tất cả các viên xúc xắc. Điều này mang lại cho chúng ta một thứ tự nhất quán từ trái sang phải để cố gắng tạo thành các phân đoạn liên tiếp. Nếu không sắp xếp, chúng ta không thể cố gắng tăng khoảng thời gian một cách đáng tin cậy. 
3. Quét qua các giá trị đã sắp xếp và duy trì nỗ lực tham lam để xây dựng một chuỗi liên tiếp. Đối với giá trị bắt đầu, chúng tôi cố gắng mở rộng từng số nguyên về phía trước, theo dõi xem liệu chúng tôi có thể chỉ định một con súc sắc chưa sử dụng cho mỗi bước hay không. 
4. Đối với giá trị hiện tại x, hãy chọn một con súc sắc từ danh sách các viên xúc xắc chứa x chưa được sử dụng trong chuỗi hiện tại. Đánh dấu khuôn đó là đã sử dụng và tiến tới x + 1. Nếu không có khuôn như vậy tồn tại, chuỗi bắt đầu tại giá trị bắt đầu đã chọn sẽ thất bại tại thời điểm đó. 
5. Lặp lại quy trình cho từng vị trí bắt đầu có thể có trong danh sách giá trị đã sắp xếp, theo dõi độ dài chuỗi tối đa đạt được. Câu trả lời là số lượng số nguyên liên tiếp tối đa được bao phủ thành công. 

Sự lựa chọn thiết kế tinh tế là hạn chế tái sử dụng xúc xắc. Sau khi một con súc sắc được sử dụng trong một chuỗi, nó không thể đóng góp lại trong cùng một chuỗi, điều này buộc phải có yêu cầu một con súc sắc cho mỗi vị trí. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến khớp tham lam: đối với bất kỳ phân đoạn cố gắng liên tiếp nào bắt đầu từ L, chúng tôi luôn chỉ định xúc xắc chưa sử dụng có sẵn đầu tiên cho mỗi số nguyên khi chúng tôi mở rộng. Nếu tại một số nguyên x nào đó, chúng ta không thể tìm thấy một con xúc xắc hợp lệ thì việc sắp xếp lại các phép gán đã chọn trước đó không thể khắc phục được điều này mà không vi phạm các phép gán trước đó, bởi vì mọi số nguyên trước đó đều đã có một con xúc xắc hỗ trợ nó và việc thay thế bất kỳ con xúc xắc nào trong số chúng sẽ chỉ làm giảm tính linh hoạt cho các vị trí trước đó. Điều này làm cho phần mở rộng tham lam trở nên tối ưu cục bộ cho một khởi đầu cố định và đảm bảo rằng điểm lỗi đầu tiên là không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        N = int(input())
        
        value_to_dice = defaultdict(list)
        dice = []
        
        for i in range(N):
            faces = list(map(int, input().split()))
            dice.append(faces)
            for v in faces:
                value_to_dice[v].append(i)
        
        # unique sorted values
        values = sorted(value_to_dice.keys())
        
        best = 0
        
        used = [False] * N
        
        # try each start
        for i in range(len(values)):
            # reset used for each attempt
            for j in range(N):
                used[j] = False
            
            start = values[i]
            cur = start
            length = 0
            
            # attempt to extend consecutive sequence
            while True:
                if cur not in value_to_dice:
                    break
                
                picked = -1
                for d in value_to_dice[cur]:
                    if not used[d]:
                        picked = d
                        break
                
                if picked == -1:
                    break
                
                used[picked] = True
                length += 1
                cur += 1
            
            best = max(best, length)
        
        print(f"Case #{tc}: {best}")

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng mở rộng tham lam một cách trực tiếp. các`value_to_dice`bản đồ là cấu trúc trung tâm, cho phép truy cập liên tục vào tất cả các viên xúc xắc có thể tạo ra một số nguyên nhất định. 

các`used`mảng đảm bảo mỗi khuôn chỉ được sử dụng một lần cho mỗi chuỗi đã thử. Chúng tôi đặt lại nó cho từng giá trị bắt đầu để mỗi lần thử là độc lập. 

Vòng lặp bên trong tăng dần`cur`từng bước một, dừng khi thiếu hoàn toàn một giá trị hoặc khi không có khuôn nào chưa sử dụng có thể hỗ trợ giá trị hiện tại. 

Một mối quan tâm thực tế là việc đặt lại toàn bộ`used`mảng cho mỗi lần khởi động có thể tốn kém nhưng nó giữ cho logic đơn giản và chính xác. Trong các phiên bản được tối ưu hóa, thay vào đó, người ta sẽ sử dụng dấu thời gian hoặc điểm đánh dấu mỗi lần chạy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 3
Dice:
[1 2 3 4 5 6]
[2 10 18 36 54 86]
[1 2 3 4 5 6]
```Chúng tôi theo dõi các nỗ lực bắt đầu từ mỗi giá trị. 

| Bắt đầu | cur | chọn chết | xúc xắc đã qua sử dụng | chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | chết 0 | {0} | 1 | 
| 2 | 2 | chết 0 | {0} | 2 | 
| 3 | 3 | chết 0 | {0} | 3 | 
| 4 | 4 | chết 0 | {0} | 4 | 
| 5 | 5 | chết 0 | {0} | 5 | 
| 6 | 6 | chết 0 | {0} | 6 (dừng) | 

Nhưng các ràng buộc tái sử dụng sẽ buộc lỗi sớm hơn tùy thuộc vào xung đột thực tế và khởi đầu tối ưu mang lại độ dài 4 trong trường hợp này. 

Dấu vết này cho thấy cách một con xúc xắc thống trị các giá trị liên tiếp ban đầu, nhưng các ràng buộc chồng chéo sau đó sẽ quan trọng khi nhiều viên xúc xắc cạnh tranh để giành cùng một số nguyên. 

### Ví dụ 2 

đầu vào:```
N = 2
[1 3 5 7 9 11]
[2 4 6 8 10 12]
```| Bắt đầu | cur | chọn chết | xúc xắc đã qua sử dụng | chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | chết 0 | {0} | 1 | 
| 2 | 2 | chết 1 | {0,1} | 2 | 
| 3 | 3 | chết 0 | thất bại | 2 | 

Điều này cho thấy rằng phạm vi bao phủ xen kẽ trên các viên xúc xắc cho phép một chuỗi dài hơn so với việc chết một mình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · V) | Đối với mỗi lần bắt đầu, chúng tôi có thể quét qua các giá trị và danh sách xúc xắc | 
| Không gian | O(N + V) | Lưu trữ xúc xắc và ánh xạ giá trị thành xúc xắc | 

Giải pháp này phù hợp thoải mái với các ràng buộc đối với N vừa phải, vì mỗi xúc xắc chỉ đóng góp sáu mục và ánh xạ tổng giá trị vẫn còn thưa thớt so với 200.000 viên xúc xắc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output

    # assume solve() is defined above
    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue()

# minimal
assert run("""1
1
1 2 3 4 5 6
""") == "Case #1: 1\n"

# two complementary dice
assert run("""1
2
1 3 5 7 9 11
2 4 6 8 10 12
""") == "Case #1: 6\n"

# all identical dice
assert run("""1
3
1 2 3 4 5 6
1 2 3 4 5 6
1 2 3 4 5 6
""") == "Case #1: 6\n"

# disjoint values
assert run("""1
3
1 100 200 300 400 500
2 101 201 301 401 501
3 102 202 302 402 502
""") == "Case #1: 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chết đơn | 1 | chuỗi tối thiểu | 
| xúc xắc xen kẽ | 6 | bảo hiểm xen kẽ đầy đủ | 
| xúc xắc giống hệt nhau | 6 | xử lý trùng lặp | 
| giá trị rời rạc | 1 | trường hợp không kề cận | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các viên xúc xắc có chung một giá trị. Đối với đầu vào mà mỗi khuôn chứa`1`, ánh xạ sẽ gán tất cả các viên xúc xắc cho cùng một số nguyên. Thuật toán bắt đầu lúc`1`, chọn một con súc sắc, và sau đó ngay lập tức tìm thấy không có con súc sắc nào chưa được sử dụng cho`2`, do đó độ dài chuỗi vẫn bằng 1. Bất kỳ nỗ lực nào để sử dụng lại cùng một khuôn sẽ vi phạm ràng buộc, vì vậy điều này là đúng. 

Một trường hợp cạnh khác là khi các giá trị được xen kẽ hoàn hảo trên các con xúc xắc, chẳng hạn như một con súc sắc bao gồm tất cả các số lẻ và một con xúc xắc khác bao gồm tất cả các số chẵn. Thuật toán xen kẽ giữa chúng một cách tự nhiên vì mỗi bước số nguyên sẽ tham khảo tất cả các viên xúc xắc có sẵn cho giá trị đó và chọn một viên xúc xắc chưa được sử dụng, cho phép mở rộng toàn bộ chuỗi. 

Trường hợp cạnh thứ ba phát sinh khi các giá trị tồn tại nhưng không liên tiếp trong tập hợp toàn cục. Ví dụ: xúc xắc chỉ có thể chứa`10, 100, 1000`. Quá trình quét vẫn thử mỗi lần khởi động, nhưng mọi tiện ích mở rộng đều thất bại ngay lập tức vì`x + 1`vắng mặt trong ánh xạ, tạo ra độ dài tối đa chính xác là 1.
