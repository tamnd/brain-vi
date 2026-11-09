---
title: "CF 1472D - Trò chơi chẵn-lẻ"
description: "Chúng ta được cấp một mảng các số nguyên và hai người chơi lần lượt loại bỏ các phần tử khỏi mảng đó. Alice bắt đầu trước. Mỗi khi người chơi loại bỏ một số, chỉ một số giá trị nhất định mới thực sự đóng góp vào điểm số của họ: Alice chỉ nhận được điểm từ các số chẵn cô ấy chọn, trong khi Bob nhận được điểm…"
date: "2026-06-11T00:31:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1472
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 693 (Div. 3)"
rating: 1200
weight: 1472
solve_time_s: 113
verified: true
draft: false
---

[CF 1472D - Trò chơi chẵn lẻ](https://codeforces.com/problemset/problem/1472/D) 

**Đánh giá:** 1200 
**Tags:** dp, trò chơi, tham lam, sắp xếp 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và hai người chơi lần lượt loại bỏ các phần tử khỏi mảng đó. Alice bắt đầu trước. Mỗi khi người chơi loại bỏ một số, chỉ một số giá trị nhất định mới thực sự đóng góp vào điểm số của họ: Alice chỉ nhận được điểm từ những số chẵn mà cô ấy chọn, trong khi Bob chỉ nhận được điểm từ những số lẻ mà anh ấy chọn. Chọn “sai chẵn lẻ” vẫn loại bỏ số nhưng không được điểm. 

Trò chơi kết thúc khi tất cả các yếu tố bị loại bỏ và người chiến thắng được quyết định bằng cách so sánh tổng số điểm. 

Khó khăn chính là cả hai người chơi không chỉ tham lam chọn cho mình. Mỗi lần loại bỏ đều ảnh hưởng đến khả năng sẵn có trong tương lai, vì vậy lối chơi tối ưu phải cân nhắc việc từ chối những cơ hội tốt của đối thủ cũng như tối đa hóa lợi ích cá nhân. 

Kích thước đầu vào đạt tới tổng số 2·10^5 phần tử trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng của tất cả các chuỗi chuyển động có thể xảy ra. Cách tiếp cận cây trò chơi đầy đủ sẽ phân nhánh ở mỗi bước và phát triển theo giai đoạn, vượt xa giới hạn khả thi. Ngay cả DP trên các tập hợp con cũng không thể thực hiện được do không gian trạng thái theo cấp số nhân. 

Điều quan trọng cần lưu ý là các quy tắc chỉ phụ thuộc vào việc một số là số chẵn hay lẻ và chỉ phụ thuộc vào giá trị của nó khi nó đóng góp vào điểm số. Điều đó có nghĩa là cấu trúc có thể giảm đi rất nhiều. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số thuộc về một lớp chẵn lẻ. Ví dụ: nếu tất cả các số đều là số lẻ, Alice sẽ không bao giờ thu được gì và Bob sẽ thu thập mọi thứ anh ấy chọn. Một cách tiếp cận ngây thơ cố gắng mô phỏng “chặn tối ưu” có thể vẫn cho rằng Alice có thể can thiệp một cách không chính xác, nhưng cô ấy không thể thay đổi quyền truy cập của Bob vào các số lẻ theo nghĩa ghi điểm có ý nghĩa. Tương tự, nếu tất cả các số đều là số chẵn thì điểm của Bob luôn bằng 0. 

Một trường hợp khác phát sinh khi các số lớn nhất thuộc về số chẵn lẻ "sai" của người chơi. Ví dụ: Alice chọn một số lẻ lớn không giúp cô ấy ghi điểm nhưng lại loại bỏ nó khỏi lợi ích tiềm năng của Bob. Điều này tạo ra một sự đánh đổi mà chỉ riêng cách tiếp cận tham lam theo giá trị có thể xử lý sai trừ khi việc phân tách chẵn lẻ là rõ ràng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô phỏng tất cả các động thái có thể xảy ra. Ở mỗi lượt, người chơi chọn bất kỳ phần tử nào còn lại, cập nhật điểm số tùy thuộc vào tính chẵn lẻ và lặp lại. Điều này mô hình hóa trò chơi một cách chính xác vì nó thử mọi con đường quyết định có thể. Tuy nhiên, mỗi trạng thái có tới O(n) lựa chọn và độ sâu n, dẫn đến khoảng n! các trình tự có thể. Ngay cả với tính năng ghi nhớ, không gian trạng thái là tập hợp nhiều phần tử còn lại với thông tin lần lượt, rất lớn về mặt thiên văn. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng giá trị thực của một số chỉ quan trọng khi nó được thu thập dưới dạng nước đi tính điểm. Nếu không, nó chỉ là một động thái chặn. Điều này gợi ý việc tách các số thành hai nhóm có thứ tự độc lập: số chẵn và số lẻ. Mỗi người chơi chỉ được hưởng lợi từ một trong những nhóm này. 

Bây giờ trò chơi trở thành một cuộc cạnh tranh xem ai sẽ chọn từ mỗi nhóm hiệu quả hơn. Vì cả hai người chơi luôn thích những cơ hội ghi điểm cao hơn nên cách chơi tối ưu sẽ giảm xuống việc xem xét thứ tự sắp xếp trong mỗi nhóm chẵn lẻ. Người chơi thay phiên nhau sử dụng các giá trị tốt nhất hiện có còn lại một cách hiệu quả, nhưng chỉ có giá trị chẵn lẻ phù hợp mới góp phần vào điểm số của họ. 

Một hiểu biết sâu sắc về cấu trúc quan trọng là tất cả các quyết định đều giảm xuống thứ tự các lượt chọn và cách chơi tối ưu tương đương với việc xử lý các số theo thứ tự giảm dần trong khi xen kẽ các lượt, với cập nhật điểm số tùy thuộc vào việc căn chỉnh vai trò chẵn lẻ. 

Điều này loại bỏ hoàn toàn sự phức tạp của cây trò chơi và thay thế nó bằng một lần duyệt được sắp xếp duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + mô phỏng tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tách mảng thành các giá trị chẵn và lẻ và sắp xếp toàn bộ mảng theo thứ tự giảm dần. 

Việc sắp xếp đảm bảo rằng cả hai người chơi luôn phải đối mặt với lựa chọn còn lại có giá trị nhất trước tiên, điều này là cần thiết vì các giá trị thấp hơn sẽ không bao giờ trở nên tốt hơn về sau. 
2. Khởi tạo hai bộ đếm điểm: AliceScore và BobScore, cả hai đều bắt đầu từ 0. 

Những hoạt động này chỉ theo dõi mức tăng hiệu quả vì các động thái không tính điểm vẫn ảnh hưởng đến tính khả dụng trong tương lai chứ không ảnh hưởng đến tổng số. 
3. Lặp lại mảng đã sắp xếp, coi mỗi vị trí là một lượt luân phiên nghiêm ngặt, bắt đầu với Alice. 

Điều này phản ánh cấu trúc chơi tối ưu: vì người chơi luôn loại bỏ một phần tử trong mỗi lượt nên thứ tự lượt chơi là cố định. 
4. Đến lượt của Alice, nếu số hiện tại là số chẵn, hãy cộng nó vào AliceScore. Nếu không thì không làm gì cả. 

Alice không bao giờ được lợi từ những số lẻ nên việc chọn chúng hoàn toàn mang tính phòng thủ. 
5. Đến lượt Bob, nếu số hiện tại là số lẻ, hãy thêm nó vào BobScore. Nếu không thì không làm gì cả. 

Bob phản chiếu Alice với tính chẵn lẻ đối lập. 
6. Sau khi xử lý xong tất cả các yếu tố, so sánh điểm số và tuyên bố người chiến thắng tương ứng. 

Ý tưởng tiềm ẩn quan trọng là cách chơi tối ưu luôn tiêu tốn các con số theo thứ tự giảm dần bất kể ưu tiên chẵn lẻ. Bất kỳ sai lệch nào cũng sẽ cho phép đối thủ nhận được giá trị cao hơn trong lượt tiếp theo của họ, điều này thực sự tệ hơn vì điểm số chỉ phụ thuộc vào giá trị thu thập được chứ không phụ thuộc vào thời gian. 

### Tại sao nó hoạt động 

Điều bất biến là sau k nước đi, k số lớn nhất còn lại đã bị xóa theo thứ tự nào đó phù hợp với lối chơi tối ưu và không người chơi nào có thể cải thiện kết quả của mình bằng cách trì hoãn hoặc bỏ qua một số có giá trị cao hơn. Vì việc ghi điểm chỉ phụ thuộc vào quyền sở hữu các lựa chọn khớp ngang bằng nên chiến lược tối ưu sẽ giảm xuống việc chỉ định xác định từng phần tử được sắp xếp cho một lượt mà không cần phải xem xét các nhánh thay thế. 

Bất kỳ nỗ lực nào để lấy một số nhỏ hơn trong khi vẫn để lại một số lớn hơn sẽ cho phép đối thủ truy cập vào số lớn hơn đó trong bước đi tiếp theo của họ, điều này chỉ có thể làm tăng điểm tốt nhất có thể đạt được của đối thủ hoặc giảm điểm của bạn. Do đó, thứ tự tham lam sẽ ổn định khi chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        alice = 0
        bob = 0
        
        for i, x in enumerate(a):
            if i % 2 == 0:
                if x % 2 == 0:
                    alice += x
            else:
                if x % 2 == 1:
                    bob += x
        
        if alice > bob:
            out.append("Alice")
        elif bob > alice:
            out.append("Bob")
        else:
            out.append("Tie")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp từng trường hợp thử nghiệm theo thứ tự giảm dần để các lựa chọn có giá trị cao nhất được giải quyết trước. Vòng lặp sau đó luân phiên sử dụng chỉ số chẵn lẻ. Các chỉ số chẵn tương ứng với Alice và các chỉ số lẻ tương ứng với Bob, kể từ khi Alice bắt đầu. Mỗi người chơi chỉ tích lũy điểm khi điều kiện chẵn lẻ phù hợp với quy tắc tính điểm của họ. 

Một chi tiết triển khai tinh tế là chúng tôi không mô phỏng việc loại bỏ một cách rõ ràng. Mảng được sắp xếp ngầm thể hiện trình tự loại bỏ trong quá trình phát tối ưu, do đó việc truyền tải chỉ mục sẽ thay thế hoàn toàn việc xóa trạng thái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
5 2 7 3
```Mảng được sắp xếp là`[7, 5, 3, 2]`. 

| Bước | Xoay | Đã chọn | Điểm Alice | BobScore | 
| --- | --- | --- | --- | --- | 
| 1 | Alice | 7 | 0 | 0 | 
| 2 | Bob | 5 | 0 | 5 | 
| 3 | Alice | 3 | 0 | 5 | 
| 4 | Bob | 2 | 0 | 5 | 

Alice không bao giờ đạt được điểm, Bob thu được 5 và 2. Bob thắng. 

Điều này chứng tỏ các giá trị lẻ lớn có giá trị hơn đối với Bob so với Alice và việc sắp xếp theo giá trị đảm bảo Bob nắm bắt được chúng khi có thể. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```Mảng được sắp xếp là`[3, 2, 1]`. 

| Bước | Xoay | Đã chọn | Điểm Alice | BobScore | 
| --- | --- | --- | --- | --- | 
| 1 | Alice | 3 | 0 | 0 | 
| 2 | Bob | 2 | 0 | 0 | 
| 3 | Alice | 1 | 0 | 0 | 

Cả hai người chơi đều không đạt được gì, dẫn đến hòa. 

Điều này nêu bật một trường hợp ranh giới trong đó tính chẵn lẻ hoàn toàn vô hiệu hóa việc ghi điểm mặc dù lối chơi không tầm thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ mảng và biến tạm thời | 

Tổng kích thước đầu vào tối đa là 2·10^5, do đó việc sắp xếp trên tất cả các trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong giới hạn. Mỗi trường hợp thử nghiệm sẽ chạy theo thời gian tuyến tính sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        
        alice = bob = 0
        
        for i, x in enumerate(a):
            if i % 2 == 0 and x % 2 == 0:
                alice += x
            elif i % 2 == 1 and x % 2 == 1:
                bob += x
        
        if alice > bob:
            out.append("Alice")
        elif bob > alice:
            out.append("Bob")
        else:
            out.append("Tie")
    
    return "\n".join(out)

# provided sample
assert run("4\n4\n5 2 7 3\n3\n3 2 1\n4\n2 2 2 2\n2\n7 8\n") == "Bob\nTie\nAlice\nAlice"

# all same parity (all even)
assert run("1\n3\n2 4 6\n") == "Alice"

# all same parity (all odd)
assert run("1\n3\n1 3 5\n") == "Bob"

# alternating parity heavy values
assert run("1\n4\n10 1 9 2\n") == "Alice"

# minimal case
assert run("1\n1\n100\n") == "Alice"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các sự kiện | Alice | Alice tích lũy tất cả lợi nhuận | 
| tất cả tỷ lệ cược | Bob | Bob tích lũy tất cả lợi nhuận | 
| cao/thấp hỗn hợp | Alice | hiệu ứng thứ tự trong việc chia điểm | 
| phần tử đơn | Alice | độ đúng cơ sở | 

## Vỏ cạnh 

Một mảng chẵn đầy đủ như`[2, 4, 6]`dẫn đến việc Alice thu thập tất cả các giá trị bất kể thứ tự. Việc sắp xếp không làm thay đổi kết quả nhưng đảm bảo việc xử lý mang tính quyết định. Điểm của Bob vẫn bằng 0 trong suốt trận đấu, vì vậy Alice thắng tổng điểm. 

Một mảng hoàn toàn lẻ như`[1, 3, 5]`mang lại cho Bob tất cả các điểm hiệu quả, vì chỉ Bob được hưởng lợi từ những lựa chọn kỳ quặc. Các nước đi của Alice chỉ loại bỏ các phần tử mà không ảnh hưởng đến điểm số, vì vậy Bob chiếm ưu thế trong tổng số cuối cùng. 

Một mảng hỗn hợp như`[10, 1, 9, 2]`thể hiện sự tương tác giữa thứ tự giá trị và gán chẵn lẻ. Sắp xếp sản lượng`[10, 9, 2, 1]`và các lượt luân phiên gán các số lẻ có giá trị cao cho Bob và các số chẵn có giá trị cao cho Alice tùy thuộc vào việc căn chỉnh lượt. Điều này khẳng định rằng thứ tự là yếu tố cấu trúc duy nhất cần thiết để dự đoán kết quả. 

Mảng một phần tử luôn thuộc về Alice vì cô ấy di chuyển trước. Nếu yếu tố chẵn là cô ấy ghi điểm, nếu không thì trò chơi sẽ kết thúc ngay lập tức với việc Bob không thể phản hồi, khiến Alice ít nhất không thua trong mọi trường hợp.
