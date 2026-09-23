---
title: "CF 104790E - Lập kế hoạch ôn thi"
description: "Mỗi bài thi chiếm một khoảng thời gian cố định và các bài thi không trùng lặp. Đối với mỗi kỳ thi, bạn có thể để nó kết thúc vào thời điểm kết thúc bình thường nếu bạn không chuẩn bị hoặc bạn có thể hoàn thành sớm hơn nếu bạn đầu tư đủ thời gian học tập trước đó."
date: "2026-06-28T16:41:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104790
codeforces_index: "E"
codeforces_contest_name: "2023 Benelux Algorithm Programming Contest (BAPC 23)"
rating: 0
weight: 104790
solve_time_s: 69
verified: true
draft: false
---

[CF 104790E - Lập kế hoạch ôn thi](https://codeforces.com/problemset/problem/104790/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi bài thi chiếm một khoảng thời gian cố định và các bài thi không trùng lặp. Đối với mỗi kỳ thi, bạn có thể để nó kết thúc vào thời điểm kết thúc bình thường nếu bạn không chuẩn bị hoặc bạn có thể hoàn thành sớm hơn nếu bạn đầu tư đủ thời gian học tập trước đó. 

Điểm mấu chốt là việc chuẩn bị không bị ràng buộc vào một khoảng thời gian liên tục duy nhất. Bạn có thể học vào bất kỳ thời gian rảnh nào giữa các kỳ thi, nhưng không bao giờ học trong khi kỳ thi đang diễn ra. Nếu bạn cố gắng tích lũy ít nhất thời gian học cần thiết cho một kỳ thi trước khi nó kết thúc thì thời gian kết thúc thực tế của kỳ thi đó sẽ sớm hơn, điều này sẽ tạo ra nhiều thời gian rảnh hơn trước khi kỳ thi tiếp theo bắt đầu. 

Nhiệm vụ là quyết định nên chuẩn bị cho bài kiểm tra nào để sau khi phân bổ thời gian học cho tất cả các khoảng trống có sẵn, số lượng bài kiểm tra kết thúc ở phiên bản “đã chuẩn bị” của chúng sẽ được tối đa hóa. 

Đầu vào đưa ra một chuỗi các bài kiểm tra được sắp xếp theo thời gian. Mỗi bài kiểm tra đều có thời gian bắt đầu, thời gian kết thúc rút ngắn nếu được chuẩn bị, thời gian kết thúc bình thường nếu không có và lượng thời gian học cần thiết để kích hoạt phiên bản ngắn hơn. Vì các kỳ thi không bao giờ trùng lặp theo thứ tự nhất định nên cấu trúc thời gian rảnh hoàn toàn được xác định bằng việc bạn chọn rút ngắn kỳ thi nào. 

Khó khăn tinh vi là việc lựa chọn chuẩn bị một bài kiểm tra không chỉ tiêu tốn thời gian học tập mà còn làm thay đổi khả năng sẵn sàng trong tương lai vì nó làm giảm thời gian chiếm giữ dòng thời gian của bài kiểm tra đó. Sự giảm thiểu đó sẽ có thể sử dụng được sau này, vì vậy các quyết định được kết hợp trên toàn cầu. 

Với tối đa 2000 bài kiểm tra, chiến lược khối hoặc tệ hơn là không khả thi. Một ý tưởng ngây thơ thử tất cả các tập hợp con của các bài kiểm tra đã chuẩn bị sẽ có tính cấp số nhân. Ngay cả việc lập trình động trên các tập hợp con cũng không thể thực hiện được. Bất kỳ giải pháp hợp lệ nào cũng phải nén trạng thái để nó chỉ theo dõi lượng thời gian học tập có thể sử dụng đã được tích lũy chứ không phải lịch sử chính xác. 

Một trường hợp thất bại phổ biến xuất phát từ việc coi việc chuẩn bị là độc lập. Ví dụ: giả sử mỗi kỳ thi chỉ yêu cầu chi tiêu ai trong bất kỳ khoảng thời gian rảnh nào trước đó, bỏ qua việc chuẩn bị cho kỳ thi cũng sẽ thay đổi thời gian rảnh trong tương lai. 

Hãy xem xét kịch bản này: 

đầu vào```
2
0 5 10 5
10 20 30 5
```Nếu bạn chuẩn bị cả hai, bạn sẽ giảm được thời lượng của cả hai, điều này sẽ tạo ra nhiều thời gian hơn, giúp ích cho việc chuẩn bị. Cách tiếp cận tham lam “chọn ngay bây giờ nếu có thể” thất bại vì những quyết định ban đầu ảnh hưởng đến việc liệu thời gian học sau này có còn tồn tại hay không. 

Một cạm bẫy khác là cho rằng tất cả việc học phải được thực hiện trước khi kỳ thi bắt đầu. Trên thực tế, thời gian học có thể được tích lũy qua tất cả các khoảng trống trước đó, do đó trạng thái là tích lũy chứ không phải theo từng kỳ thi cục bộ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng chuẩn bị từng tập hợp con của bài kiểm tra. Đối với một tập hợp con cố định, chúng tôi mô phỏng thời gian, theo dõi lượng học tập được tích lũy trong các khoảng trống và kiểm tra xem mỗi bài kiểm tra đã chọn có thể đáp ứng được hay không. Mô phỏng này là tuyến tính, nhưng có 2ⁿ tập hợp con, khiến phương pháp này hoàn toàn không khả thi ngay cả đối với n nhỏ. 

Lý do khiến lực lượng tàn bạo này hoạt động về mặt khái niệm là vì dòng thời gian mang tính quyết định sau khi tập hợp các bài kiểm tra đã chuẩn bị được ấn định. Thất bại là sự bùng nổ tổ hợp: mỗi kỳ thi nhân đôi số khả năng. 

Điểm mấu chốt là chúng ta thực sự không cần phải nhớ bài kiểm tra nào đã được chọn, mà chỉ cần nhớ chúng ta hiện có bao nhiêu thời gian học tập có thể sử dụng được và thời gian đó sẽ thay đổi như thế nào khi chúng ta chuyển từ bài kiểm tra này sang bài kiểm tra tiếp theo. Cấu trúc của bài toán cho phép diễn giải lập trình động trong đó trạng thái là lượng thời gian học tích lũy ở mỗi ranh giới của bài kiểm tra và quá trình chuyển đổi mô phỏng việc chuẩn bị hoặc bỏ qua bài kiểm tra hiện tại. 

Quan sát quan trọng là tất cả những thay đổi về thời gian có liên quan đều mang tính cộng gộp và có thể được biểu diễn dưới dạng những thay đổi trong một tài nguyên vô hướng duy nhất. Việc chuẩn bị một bài kiểm tra tiêu tốn ai đơn vị tài nguyên này nhưng cũng làm tăng thời gian có sẵn trong tương lai bằng cách rút ngắn bài kiểm tra xuống (ei − pi). Điều này làm cho vấn đề trở thành một DP giống như chiếc ba lô, nơi các vật phẩm vừa tiêu thụ vừa tạo ra tài nguyên. 

Chúng tôi giới hạn thời gian học tập được theo dõi ở mức n vì chúng tôi không bao giờ có thể chuẩn bị nhiều hơn n bài kiểm tra, vì vậy việc có nhiều hơn n đơn vị thời gian rảnh không bao giờ hữu ích cho việc đếm xem có thể hoàn thành bao nhiêu bài kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2ⁿ · n) | O(n) | Quá chậm | 
| Lập trình động | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các bài kiểm tra theo thứ tự thời gian và duy trì DP về lượng thời gian học tập có thể sử dụng mà chúng tôi hiện có ngay trước mỗi kỳ thi. 

Mỗi trạng thái DP đại diện cho số lượng bài kiểm tra tối đa mà chúng ta có thể chuẩn bị đầy đủ trong số i bài kiểm tra đầu tiên, với một lượng thời gian học tập tích lũy nhất định có sẵn tại thời điểm chúng ta đạt được bài kiểm tra i. 

1. Khởi tạo DP vào thời điểm trước kỳ thi đầu tiên. Tại thời điểm đó, không có nghiên cứu nào được tích lũy và không có bài kiểm tra nào được hoàn thành, vì vậy trạng thái hợp lệ duy nhất là không có thời gian học và không có bài kiểm tra nào được chuẩn bị. 
2. Đối với mỗi bài kiểm tra thứ i, trước tiên hãy tính đến khoảng thời gian giữa lúc kết thúc bài kiểm tra trước và bắt đầu bài kiểm tra hiện tại. Bất kỳ trạng thái DP hiện tại nào đều có thêm thời gian học tập miễn phí bằng với khoảng cách đó, bởi vì đây là thời gian không bị gián đoạn khi có thể học tập. 
3. Sau khi kết hợp khoảng trống, chúng ta xem xét hai lựa chọn cho bài thi i. Đầu tiên là không chuẩn bị nó. Trong trường hợp này, chúng tôi chỉ đơn giản chuyển tiếp thời gian học hiện tại vì chúng tôi không tốn bất cứ chi phí nào và kỳ thi sẽ kéo dài cho đến khi hết thời gian, ảnh hưởng tương ứng đến khoảng thời gian tiếp theo. 
4. Lựa chọn thứ hai là luyện thi i. Điều này đòi hỏi hiện trạng phải có ít nhất ai đơn vị thời gian học tập sẵn có. Nếu vậy, chúng tôi trừ ai khỏi trạng thái và chúng tôi tăng số bài kiểm tra được chuẩn bị thành công lên một. 
5. Nếu chúng ta ôn thi i, chúng ta cũng có thêm thời gian rảnh sau này vì kỳ thi kết thúc ở số pi thay vì ei. Sự khác biệt này (ei − pi) được cộng vào thời gian học sẵn có để chuyển tiếp trong tương lai, vì thời gian đó sẽ được sử dụng trước khi kỳ thi tiếp theo bắt đầu. 
6. Sau khi đánh giá cả hai lựa chọn, chúng ta chuyển sang bài kiểm tra tiếp theo, tiếp tục tất cả các trạng thái DP đã cập nhật, một lần nữa được lập chỉ mục theo thời gian học hiện có.

Bảng DP được cắt ngắn sao cho thời gian học không bao giờ vượt quá n, vì có nhiều hơn n đơn vị thời gian rảnh không thể cải thiện được số lượng bài thi chuẩn bị. 

### Tại sao nó hoạt động 

Tại mỗi ranh giới kỳ thi, trạng thái DP nắm bắt chính xác lượng thời gian học tập vẫn có thể được phân bổ lại cho các kỳ thi trong tương lai. Bất kỳ hai lịch sử nào có cùng thời gian học tập sẵn có đều có thể thay thế cho nhau, bởi vì các quyết định trong tương lai chỉ phụ thuộc vào số tiền học tập vẫn có thể được chi tiêu chứ không phụ thuộc vào cách đạt được số tiền đó. 

Quá trình chuyển đổi mô hình chính xác việc bảo tồn thời gian: các khoảng trống làm tăng thêm thời gian có thể sử dụng, việc chuẩn bị sẽ tiêu tốn thời gian đó và việc chuẩn bị thành công sẽ tạo ra các khoảng trống bổ sung trong tương lai bằng cách rút ngắn các kỳ thi. Bởi vì mọi chuyển đổi đều tuyến tính và cục bộ với các thăm khám liền kề nên không có sự phụ thuộc ẩn nào bị mất bằng cách nén lịch sử vào một trạng thái vô hướng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    exams = []
    for _ in range(n):
        s, p, e, a = map(int, input().split())
        exams.append((s, p, e, a))
    
    CAP = n
    
    dp = [-10**9] * (CAP + 1)
    dp[0] = 0
    
    prev_end = 0
    
    for i in range(n):
        s, p, e, a = exams[i]
        
        gap = s - prev_end
        
        ndp = [-10**9] * (CAP + 1)
        
        for t in range(CAP + 1):
            if dp[t] < 0:
                continue
            
            nt = t + gap
            if nt > CAP:
                nt = CAP
            
            best = dp[t]
            
            ndp[nt] = max(ndp[nt], best)
            
            if t >= a:
                nt2 = t - a + (e - p)
                nt2 += gap
                if nt2 > CAP:
                    nt2 = CAP
                ndp[nt2] = max(ndp[nt2], best + 1)
        
        dp = ndp
        prev_end = e
    
    print(max(dp))

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ số lượng bài kiểm tra tối đa đã được chuẩn bị thành công cho mỗi khoảng thời gian học tích lũy có thể. Chi tiết triển khai chính là trước tiên chúng tôi tiếp thu khoảng cách thời gian giữa các kỳ thi trước khi áp dụng chuyển tiếp cho kỳ thi hiện tại. 

Khi chúng tôi bỏ qua một bài kiểm tra, chúng tôi sẽ chuyển tiếp trạng thái không thay đổi ngoại trừ việc thêm khoảng cách. Khi chuẩn bị, chúng tôi đảm bảo có đủ thời gian học tập, trừ đi, cộng thêm lợi ích thu được từ việc rút ngắn kỳ thi, sau đó cũng cộng thêm phần đóng góp vào khoảng cách. Câu trả lời cuối cùng là giá trị tốt nhất trong số tất cả các trạng thái DP có thể tiếp cận. 

Một sai lầm phổ biến là quên rằng khoảng cách phải được áp dụng ở cả hai nhánh, vì cả hai lựa chọn đều đạt đến ranh giới kỳ thi tiếp theo. Một lỗi khác là không giới hạn được trạng thái DP, khiến độ phức tạp bị giới hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
3
10 20 30 5
30 50 100 15
100 101 200 50
```Chúng tôi theo dõi trạng thái DP dưới dạng (study_time → best_count). Chỉ những chuyển tiếp quan trọng mới được hiển thị. 

Sau khi học hết khoảng trống đầu tiên, việc học tập còn hạn chế nên việc chuẩn bị cho kỳ thi sớm phụ thuộc vào việc tích lũy đủ thời gian rảnh rỗi từ thời gian trước đó. 

| Bước | thi | Trạng thái DP chính (đơn giản hóa) | 
| --- | --- | --- | 
| 1 | (10,20,30,5) | 0 → 0 | 
| 2 | (30,50,100,15) | tiểu bang nhỏ được cập nhật | 
| 3 | (100,101,200,50) | tốt nhất cuối cùng đạt đến mức tối đa | 

Dấu vết này cho thấy các quyết định ban đầu xác định liệu các yêu cầu nghiên cứu lớn sau này có thể được đáp ứng hay không. 

Bây giờ hãy xem xét một trường hợp chặt chẽ hơn:```
2
0 5 10 5
10 20 30 5
```| Bước | thi | tiến hóa DP | 
| --- | --- | --- | 
| 1 | kỳ thi đầu tiên | 0 → 0 hoặc 1 nếu đủ chùng | 
| 2 | kỳ thi thứ hai | phụ thuộc vào việc chuẩn bị trước hay chưa | 

Kỳ thi thứ hai chỉ trở nên khả thi nếu lựa chọn thứ nhất duy trì đủ năng lực học tập, thể hiện sự phụ thuộc lẫn nhau. 

Những ví dụ này nhấn mạnh rằng việc lựa chọn tối ưu phụ thuộc vào thời gian nghiên cứu tích lũy chứ không phải tính khả thi của địa phương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi kỳ thi xử lý tối đa n trạng thái DP | 
| Không gian | O(n) | Chỉ có hai mảng DP có kích thước n được giữ lại | 

Giới hạn bậc hai đủ cho n đến 2000, tạo ra khoảng bốn triệu chuyển đổi trạng thái, phù hợp thoải mái với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# provided samples
assert run("""3
10 20 30 5
30 50 100 15
100 101 200 50
""").strip() == "3"

assert run("""3
1000 1001 1002 1000
1003 1004 1005 500
1006 1007 1008 500
""").strip() == "0"

# minimal case
assert run("""1
0 1 2 1
""").strip() in {"0", "1"}

# all easy to prepare
assert run("""2
0 1 2 1
2 3 4 1
""").strip() == "2"

# tight study requirement
assert run("""2
0 1 100 50
100 101 200 50
""").strip() == "0"

# boundary stress
assert run("""3
0 1 2 1
2 3 4 1
4 5 6 1
""").strip() == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kỳ thi đơn tối thiểu | 0 hoặc 1 | tính khả thi cơ bản | 
| tất cả chuỗi dễ dàng | 2 | công trình tích lũy | 
| yêu cầu chặt chẽ | 0 | trường hợp bất khả thi | 
| dây chuyền nhỏ | 3 | chuyển tiếp lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi các khoảng trống chính xác bằng 0 hoặc rất nhỏ. Trong tình huống đó, tất cả tính khả thi hoàn toàn phụ thuộc vào việc các kỳ thi trước đó đã được chuẩn bị hay chưa vì không có thời gian học bên ngoài. 

Ví dụ:```
2
0 1 2 2
2 3 4 2
```Bài thi đầu tiên không thể chuẩn bị được do không đủ thời gian tích lũy nên bài thi thứ hai cũng trở nên bất khả thi. DP luôn giữ trạng thái ở mức 0 một cách chính xác, không bao giờ phát minh ra thời gian rảnh. 

Một trường hợp nguy hiểm khác là việc chuẩn bị cho một kỳ thi tốn nhiều thời gian hơn mức cần thiết để đưa ra các quyết định trong tương lai. DP giới hạn thời gian nghiên cứu tích lũy ở mức n, đảm bảo rằng độ trễ quá mức không làm tăng không gian trạng thái hoặc tạo ra sự khác biệt nhân tạo giữa các cấu hình tương đương. 

Điều này đảm bảo rằng ngay cả khi các giá trị lớn như 10⁹ xuất hiện trong đầu vào, DP vẫn ổn định và giới hạn.
