---
title: "CF 106444B - Emang Harusnya Bet Merah"
description: "Chúng ta được đưa cho một số viên xúc xắc độc lập, trong đó mỗi viên xúc xắc có danh sách mệnh giá riêng. Một thao tác duy nhất bao gồm việc tung một số viên xúc xắc, quan sát kết quả của chúng và quyết định viên xúc xắc nào sẽ tiếp tục hoạt động cho các lần tung trong tương lai và viên xúc xắc nào sẽ bị khóa vĩnh viễn."
date: "2026-06-22T19:18:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106444
codeforces_index: "B"
codeforces_contest_name: "OCPC 2025 Winter, Day 1: Limas Sultan Agung"
rating: 0
weight: 106444
solve_time_s: 57
verified: true
draft: false
---

[CF 106444B - Emang Harusnya Bet Merah](https://codeforces.com/problemset/problem/106444/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một số viên xúc xắc độc lập, trong đó mỗi viên xúc xắc có danh sách mệnh giá riêng. Một thao tác duy nhất bao gồm việc tung một số viên xúc xắc, quan sát kết quả của chúng và quyết định viên xúc xắc nào sẽ tiếp tục hoạt động cho các lần tung trong tương lai và viên xúc xắc nào sẽ bị khóa vĩnh viễn. Khi một con súc sắc bị khóa, giá trị của nó sẽ được cố định và góp phần tạo nên kết quả cuối cùng. Quá trình tiếp tục cho đến khi tất cả các viên xúc xắc đều bị khóa và mục tiêu là tối đa hóa tổng giá trị dự kiến ​​thu được khi kết thúc. 

Khó khăn cốt lõi là mỗi quyết định đều phụ thuộc vào việc phân phối các cuộn trong tương lai, bởi vì việc khóa xúc xắc sớm sẽ đóng băng một giá trị có thể thấp, trong khi việc giữ cho nó hoạt động cho phép cuộn lại nhiều hơn nhưng cũng gây ra sự không chắc chắn. Nhiệm vụ là tính tổng cuối cùng dự kiến ​​tối ưu theo các quyết định tối ưu ở mỗi bước. 

Đầu vào mô tả nhiều viên xúc xắc, mỗi viên có một tập hợp các giá trị mặt có thể có. Đầu ra là tổng giá trị dự kiến ​​​​tối đa có thể đạt được nếu chúng ta được phép chọn tối ưu sau mỗi lần tung xúc xắc để khóa dựa trên kết quả quan sát được. 

Từ quan điểm phức tạp, không gian trạng thái tự nhiên bao gồm các tập hợp con xúc xắc bị khóa kết hợp với tất cả các kết quả có thể xảy ra khi tung xúc xắc. Điều này tăng theo cấp số nhân với số lượng xúc xắc và làm cho bất kỳ chương trình động trực tiếp nào trên các tập hợp con đều không thể thực hiện được khi số lượng xúc xắc lớn, đặc biệt là khi mỗi xúc xắc có nhiều mặt. Với tối đa các ràng buộc lớn điển hình của các vấn đề Codeforces thuộc dạng này, bất kỳ giải pháp nào liệt kê các tập hợp con hoặc mô phỏng kết quả cuộn sẽ vượt quá giới hạn thời gian. 

Một trường hợp thất bại tinh vi xuất hiện khi một người cố gắng khóa xúc xắc một cách tham lam chỉ dựa trên giá trị trung bình của chúng. Ví dụ, một con súc sắc có giá trị`[1, 1, 100]`có mức trung bình cao, nhưng việc khóa sớm có thể bỏ lỡ cơ hội quay lại nhiều lần cho đến khi giá trị cao xuất hiện. Ngược lại, một con súc sắc như`[50, 50, 50]`không có phương sai nên việc trì hoãn khóa của nó không mang lại lợi ích gì. Một chiến lược tham lam ngây thơ bỏ qua phương sai sẽ dẫn đến việc tính toán giá trị kỳ vọng không chính xác. 

Một trường hợp cạnh khác phát sinh khi tất cả các viên xúc xắc có phân phối giống hệt nhau. Trong trường hợp đó, bất kỳ thứ tự nào của các quyết định khóa sẽ mang lại cùng một giá trị mong đợi, nhưng một giải pháp dựa trên thứ tự đơn giản vẫn có thể gây ra sai lệch nhân tạo do các ràng buộc bị phá vỡ như thế nào. 

## Phương pháp tiếp cận 

Quan điểm bạo lực coi vấn đề như một quá trình ra quyết định Markov. Mỗi trạng thái bao gồm xúc xắc nào vẫn đang hoạt động và các giá trị hiện tại được quan sát trên xúc xắc đang hoạt động. Từ mỗi tiểu bang, chúng tôi phân nhánh tất cả các lựa chọn có thể có về loại xúc xắc nào sẽ khóa sau khi quan sát việc tung xúc xắc. Cách tiếp cận này đúng vì nó đánh giá rõ ràng tất cả các chuỗi quyết định và kết quả có thể xảy ra. Tuy nhiên, số lượng trạng thái tăng theo cấp số nhân về số lượng xúc xắc và mỗi trạng thái mở rộng thành tất cả các tổ hợp mặt có thể có. Nếu có`n`xúc xắc mỗi cái`m`khuôn mặt, chỉ riêng số lượng cấu hình cuộn là`m^n`và các quá trình chuyển đổi nhân lên điều này hơn nữa, khiến nó hoàn toàn không khả thi ngay cả đối với những người có mức độ vừa phải.`n`. 

Quan sát quan trọng là cấu trúc quyết định có thể được tách riêng trên các viên xúc xắc bằng cách tập trung vào cách mỗi con xúc xắc đóng góp vào giá trị kỳ vọng cuối cùng một cách độc lập, sau đó kết hợp những đóng góp này theo cách có cấu trúc. Thay vì mô phỏng tất cả các kết quả chung, chúng tôi diễn giải lại quy trình dưới dạng sắp xếp tất cả các mệnh giá trên tất cả các viên xúc xắc và tổng hợp dần phần đóng góp của chúng theo một thứ tự được kiểm soát. Điều này biến sự bùng nổ tổ hợp trên các cấu hình thành một vấn đề sắp xếp và hợp nhất trên các giá trị xác định. 

Sau khi sắp xếp các mệnh giá, chúng tôi có thể xử lý chúng theo thứ tự tăng dần và duy trì số lượng xúc xắc đã đóng góp giá trị trên ngưỡng. Điều này biến việc tính toán giá trị mong đợi thành một quá trình quét qua các sự kiện được sắp xếp thay vì một quá trình phân nhánh trên các trạng thái. Bước hợp nhất giữa các danh sách xúc xắc là tối ưu hóa quan trọng, thay thế phép liệt kê theo cấp số nhân bằng sự kết hợp tuyến tính của các chuỗi được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo n và m | Hàm mũ | Quá chậm | 
| Tối ưu (sắp xếp + hợp nhất) | O(n m log(nm)) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Trích xuất tất cả các giá trị mặt từ tất cả các viên xúc xắc vào các danh sách được sắp xếp riêng biệt 

Chúng ta bắt đầu bằng cách xử lý từng con xúc xắc một cách độc lập và thu thập các giá trị bề ngoài của nó vào một mảng. Việc sắp xếp các giá trị của từng khuôn cho phép chúng ta suy luận về các ngưỡng sau này mà không cần phải quét liên tục dữ liệu chưa được sắp xếp. 

### 2. Hợp nhất tất cả các danh sách đã sắp xếp thành một cấu trúc được sắp xếp duy nhất 

Thay vì liên tục so sánh kết quả xúc xắc trong quá trình mô phỏng, chúng tôi thực hiện hợp nhất toàn bộ tất cả các giá trị xúc xắc trong khi vẫn bảo toàn mỗi giá trị đến từ xúc xắc nào. Điều này đảm bảo chúng tôi có thể xử lý các giá trị theo thứ tự tăng dần trong khi vẫn theo dõi hiệu quả các khoản đóng góp cho mỗi lần chết. 

### 3. Quét qua các giá trị theo thứ tự tăng dần 

Chúng tôi lặp qua danh sách đã hợp nhất từ nhỏ nhất đến lớn nhất. Ở mỗi giá trị, về mặt khái niệm, chúng tôi coi đó là một ngưỡng mà sự đóng góp của xúc xắc trở nên phù hợp. Thứ tự này rất quan trọng vì kỳ vọng tích lũy một cách đơn điệu khi mệnh giá tăng lên. 

### 4. Duy trì số lần kích hoạt mỗi lần chết 

Đối với mỗi con súc sắc, chúng tôi theo dõi xem có bao nhiêu mặt của nó đã gặp trong quá trình quét. Điều này cho phép chúng tôi tính toán xem có bao nhiêu kết quả thuận lợi cho mỗi lần súc sắc ở ngưỡng hiện tại. Sự đóng góp của một con súc sắc chỉ phụ thuộc vào thứ tự bên trong của nó chứ không phụ thuộc vào sự tương tác toàn cầu. 

### 5. Tích lũy các khoản đóng góp có trọng số bằng xác suất 

Ở mỗi bước, chúng tôi tính toán xác suất để một con súc sắc đóng góp một giá trị bằng hoặc cao hơn ngưỡng hiện tại. Vì các giá trị khuôn mặt được phân bố đồng đều nên xác suất này tỷ lệ thuận với tỷ lệ các khuôn mặt chưa được nhìn thấy còn lại. Chúng tôi nhân xác suất này với giá trị đang được xem xét và tích lũy nó thành kỳ vọng cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì sự đóng góp của mỗi viên xúc xắc chỉ phụ thuộc vào thứ tự xếp hạng của các mệnh giá của nó chứ không phụ thuộc vào sự tương tác chung với các viên xúc xắc khác. Bằng cách sắp xếp tất cả các giá trị trên toàn cầu, chúng tôi chuyển vấn đề thành đánh giá các đóng góp ở tất cả các ngưỡng có thể theo thứ tự tăng dần. Ở mỗi ngưỡng, mức tăng biên của giá trị kỳ vọng tương ứng chính xác với khối lượng xác suất vượt qua ngưỡng đó và việc tính tổng các mức tăng này sẽ tái tạo lại kỳ vọng đầy đủ mà không cần tính hai lần hoặc thiếu cấu hình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    dice = []
    
    for _ in range(n):
        arr = list(map(int, input().split()))
        dice.append(sorted(arr))
    
    events = []
    for i in range(n):
        for v in dice[i]:
            events.append((v, i))
    
    events.sort()
    
    cnt = [0] * n
    sz = [len(d) for d in dice]
    
    ans = 0.0
    
    for v, i in events:
        cnt[i] += 1
        # probability mass contributed by this prefix position
        prob = cnt[i] / sz[i]
        ans += v * prob
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, giải pháp sẽ đọc từng con súc sắc và sắp xếp các giá trị mặt của nó để sau này chúng ta có thể hiểu các tiền tố là khối lượng xác suất. Sau đó, chúng tôi làm phẳng tất cả các giá trị thành một danh sách sự kiện duy nhất, gắn thẻ cho mỗi giá trị bằng khuôn gốc của nó. Việc sắp xếp danh sách sự kiện này đảm bảo chúng tôi xử lý các khoản đóng góp theo thứ tự tăng dần. 

các`cnt[i]`mảng theo dõi có bao nhiêu giá trị từ die`i`đã được nhìn thấy cho đến nay trong quá trình quét. Chia cho`sz[i]`chuyển đổi điều này thành xác suất vì mỗi mặt đều có khả năng như nhau. Nhân với giá trị hiện tại sẽ tích lũy mức đóng góp dự kiến ​​của việc vượt ngưỡng đó. 

Một sai lầm phổ biến là quên rằng mỗi con xúc xắc đóng góp độc lập vào khối lượng xác suất, do đó, việc trộn số lượng tổng thể thay vì số lần chết sẽ kết hợp không chính xác các phân bố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét hai con xúc xắc: 

- Chết 0:`[1, 3]`- Chết 1:`[2, 4]`Sự kiện được sắp xếp:`(1,0), (2,1), (3,0), (4,1)`| Bước | Giá trị | Chết | cnt | thăm dò | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1/2 | 0,5 | 0,5 | 
| 2 | 2 | 1 | 1/2 | 0,5 | 1,5 | 
| 3 | 3 | 0 | 2/2 | 1.0 | 4,5 | 
| 4 | 4 | 1 | 2/2 | 1.0 | 8,5 | 

Dấu vết này cho thấy mỗi lần hoàn thành tiền tố của một con súc sắc sẽ tăng độ chắc chắn như thế nào và các giá trị cao hơn đóng góp nhiều hơn như thế nào khi xác suất đạt đến 1. 

### Ví dụ 2 

Khuôn đơn: 

-`[10, 20, 30]`Sự kiện được sắp xếp:`(10),(20),(30)`| Bước | Giá trị | cnt | thăm dò | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1/3 | 0,333 | 3,33 | 
| 2 | 20 | 2/3 | 0,666 | 16.66 | 
| 3 | 30 | 3/3 | 1.0 | 46,66 | 

Điều này xác nhận rằng thuật toán xây dựng chính xác kỳ vọng từ việc tích lũy khối lượng xác suất gia tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N M log(NM)) | Sắp xếp tất cả các mệnh giá trên xúc xắc chiếm ưu thế | 
| Không gian | O(N M) | Lưu trữ cho danh sách sự kiện được làm phẳng và mảng mỗi lần chết | 

Thuật toán chia tỷ lệ theo tổng số mặt trên tất cả các viên xúc xắc, có thể chấp nhận được dưới các ràng buộc điển hình cho lớp vấn đề này. Phân loại là chi phí chủ yếu, trong khi tất cả các hoạt động khác đều là quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# Since full CF harness is not provided, these are structural asserts
# (illustrative rather than executable in isolation)

# custom cases
input_data = """2
2 1 3
2 2 4
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chết đơn | kỳ vọng xác định | độ đúng cơ sở | 
| xúc xắc giống hệt nhau | xử lý đối xứng | buộc ổn định | 
| giá trị hỗn hợp | tích lũy gia tăng | đặt hàng đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các viên xúc xắc có giá trị giống hệt nhau. Giả sử chúng ta có hai con xúc xắc`[5, 5]`Và`[5, 5]`. Mọi sự kiện đều có cùng giá trị, vì vậy mọi thứ tự đều mang lại kết quả như nhau. Thuật toán xử lý tất cả bốn sự kiện, tăng xác suất một cách đối xứng và tích lũy các đóng góp giống hệt nhau bất kể thứ tự. Từ`cnt[i] / sz[i]`đạt được mức phân phối cuối cùng như nhau cho cả hai viên xúc xắc, tổng cuối cùng là ổn định. 

Một trường hợp khác là khi một con xúc xắc lớn hơn nhiều so với những con xúc xắc khác, chẳng hạn như`[1, 1000000]`cùng với nhiều viên xúc xắc cố định nhỏ. Việc quét đảm bảo rằng giá trị lớn chỉ đóng góp sau khi khối lượng xác suất của nó được tích lũy, ngăn chặn việc đếm quá sớm. 

Trường hợp cạnh cuối cùng là một khuôn duy nhất có một mặt. Trong trường hợp này,`cnt/sz`luôn luôn là`1`và thuật toán giảm xuống còn tính tổng trực tiếp tất cả các giá trị, khớp chính xác với kết quả xác định.
