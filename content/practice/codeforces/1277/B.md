---
title: "CF 1277B - Làm Họ Kỳ Lạ"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với danh sách các số nguyên dương. Một thao tác cho phép chúng ta chọn một giá trị $c$, nhưng chỉ khi nó là số chẵn, và sau đó chúng ta đồng thời thay thế mọi lần xuất hiện của $c$ trong mảng bằng $c/2$."
date: "2026-06-16T01:52:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1277
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 606 (Div. 2, based on Technocup 2020 Elimination Round 4)"
rating: 1200
weight: 1277
solve_time_s: 590
verified: true
draft: false
---

[CF 1277B - Khiến họ trở nên kỳ quặc](https://codeforces.com/problemset/problem/1277/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, lý thuyết số 
**Thời gian giải:** 9 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi bắt đầu với danh sách các số nguyên dương. Một thao tác cho phép chúng ta chọn một giá trị$c$, nhưng chỉ khi nó chẵn và sau đó chúng ta đồng thời thay thế mọi lần xuất hiện của$c$trong mảng bởi$c/2$. 

Mục tiêu là lặp lại thao tác này ít lần nhất có thể cho đến khi mọi số trong mảng trở thành số lẻ. 

Một cách hữu ích để diễn giải quy trình là mỗi giá trị phát triển độc lập bằng cách chia đôi nhiều lần, nhưng hạn chế chính là chúng tôi không thể nhắm mục tiêu vào một vị trí riêng lẻ, chúng tôi chỉ ảnh hưởng đến tất cả các lần xuất hiện của một giá trị chẵn đã chọn cùng một lúc. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số trên tất cả các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào cố gắng mô phỏng từng bước thao tác trên toàn bộ mảng hoặc quét liên tục mảng cho mỗi thao tác sẽ không vượt qua. Về cơ bản, một giải pháp phải xử lý từng phần tử với số lần không đổi hoặc nén cấu trúc lặp lại thành một lần vượt qua cho mỗi trường hợp thử nghiệm. 

Một trường hợp phức tạp xuất hiện khi nhiều số có chung các giá trị trung gian chồng chéo trong chuỗi giảm một nửa của chúng. Ví dụ,$40$Và$20$tương tác vì$40 \to 20 \to 10 \to 5$trong khi$20 \to 10 \to 5$. Một cách tiếp cận ngây thơ xử lý các phần tử một cách độc lập sẽ đánh giá thấp chi phí vì nó bỏ qua rằng các giá trị trung gian được chia sẻ vẫn yêu cầu các hoạt động riêng biệt trừ khi được tính toán cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng quá trình một cách trực tiếp. Mặc dù tồn tại ít nhất một giá trị chẵn trong mảng, chúng tôi chọn một số giá trị chẵn$c$, chia tất cả các lần xuất hiện của$c$bằng hai và lặp lại cho đến khi tất cả các giá trị trở thành số lẻ. Mỗi bước yêu cầu quét hoặc duy trì bản đồ tần số của các giá trị để quyết định số chẵn nào sẽ hoạt động. Trong trường hợp xấu nhất, các giá trị như$2^k$tạo ra chuỗi chia đôi dài và nhiều giá trị riêng biệt có thể xuất hiện lại nhiều lần sau khi biến đổi. Điều này dẫn tới khả năng$O(n \log A)$hoạt động và mỗi hoạt động có thể yêu cầu cập nhật số lượng trên một cấu trúc lớn, khiến nó quá chậm. 

Quan sát quan trọng là mỗi số đóng góp một “chuỗi” các giá trị chẵn mà nó đi qua trước khi trở thành số lẻ. Đối với một số$x$, chúng ta có thể chia đôi nhiều lần cho đến khi nó trở thành số lẻ, ghi lại tất cả các giá trị chẵn trung gian dọc theo đường đi. Ví dụ,$40$sản xuất chuỗi$40, 20, 10$. Mỗi giá trị riêng biệt trong chuỗi này biểu thị một thao tác bắt buộc tại một số điểm: ít nhất một thao tác phải được thực hiện cho mỗi số chẵn riêng biệt xuất hiện trong bất kỳ chuỗi nào. 

Bây giờ cấu trúc quan trọng trở nên rõ ràng: câu trả lời chính xác là số lượng số nguyên chẵn riêng biệt xuất hiện ở bất kỳ đâu trong bất kỳ chuỗi chia đôi nào của mảng. Mỗi giá trị như vậy phải được chọn ít nhất một lần và việc chọn nó một lần là đủ vì thao tác đó xử lý đồng thời tất cả các lần xuất hiện của giá trị đó trên tất cả các phần tử. 

Điều này làm giảm vấn đề thu thập tất cả các giá trị chẵn trung gian trên tất cả các số và đếm các giá trị duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n \cdot \text{operations})$|$O(n)$| Quá chậm | 
| Theo dõi các giá trị chuỗi chẵn duy nhất |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi test case, tạo một bộ trống để lưu trữ các giá trị tương ứng với các thao tác được yêu cầu. Bộ này đại diện cho tất cả các số chẵn xuất hiện trong bất kỳ chuỗi giảm một nửa nào. 
2. Lặp lại từng số trong mảng. 
3. Trong khi số hiện tại là số chẵn, hãy chèn nó vào bộ và chia cho hai. Điều này mô phỏng theo quỹ đạo của số cho đến khi nó trở thành số lẻ. Mỗi dấu chèn cho biết chúng ta sẽ cần ít nhất một thao tác nhắm tới giá trị đó. 
4. Tiếp tục quá trình này một cách độc lập cho tất cả các số. 
5. Sau khi xử lý tất cả các phần tử, kích thước của tập hợp là câu trả lời cho ca kiểm thử. 

Lý do chúng tôi chỉ ghi số chẵn là vì số lẻ đã ở dạng cuối cùng và không bao giờ yêu cầu thực hiện phép tính. 

### Tại sao nó hoạt động 

Mỗi thao tác nhắm đến một giá trị chẵn duy nhất và giảm tất cả các lần xuất hiện của nó cùng một lúc. Do đó, mọi giá trị chẵn riêng biệt xuất hiện ở bất kỳ giai đoạn nào trong quá trình giảm một nửa của bất kỳ phần tử nào đều phải bị loại bỏ ít nhất một lần. Ngược lại, thực hiện một thao tác trên mỗi giá trị chẵn riêng biệt là đủ vì một khi chúng ta xử lý một giá trị$c$, tất cả các lần xuất hiện của$c$ngay lập tức trở thành$c/2$và chúng tôi không bao giờ cần xử lý$c$lại. Điều này tạo ra sự tương ứng một-một giữa các giá trị chẵn riêng biệt gặp phải và các bước di chuyển cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        seen = set()
        
        for x in a:
            while x % 2 == 0:
                seen.add(x)
                x //= 2
        
        print(len(seen))

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại qua từng số và liên tục chia số đó cho hai trong khi số đó vẫn là số chẵn. Mỗi giá trị chẵn trung gian được lưu trữ trong một tập hợp sao cho các giá trị trùng lặp trong mảng không làm sai lệch câu trả lời. Kết quả cuối cùng chỉ đơn giản là số lượng giá trị chẵn duy nhất gặp phải. 

Một lỗi phổ biến là dừng lại ở lần xuất hiện đầu tiên của một giá trị hoặc đếm từng phần tử thay vì tính tổng thể. Điều đó sẽ đếm quá hoặc thiếu vì nhiều phần tử có thể có chung một phần tử tổ tiên chẵn và mỗi phần tử tổ tiên như vậy tương ứng với một thao tác bắt buộc duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:$[40, 6, 40, 3, 20, 1]$| Yếu tố | Sản xuất dây chuyền chẵn | Đặt sau khi xử lý | 
| --- | --- | --- | 
| 40 | 40, 20, 10 | {40, 20, 10} | 
| 6 | 6, 3 | {40, 20, 10, 6} | 
| 40 | 40, 20, 10 | {40, 20, 10, 6} | 
| 3 | không | {40, 20, 10, 6} | 
| 20 | 20, 10 | {40, 20, 10, 6} | 
| 1 | không | {40, 20, 10, 6} | 

Câu trả lời cuối cùng là 4. 

Điều này cho thấy việc lặp đi lặp lại của cùng một chuỗi halving không làm tăng số lượng thao tác cần thiết. Bộ nén tất cả các phần dư thừa. 

### Ví dụ 2 

Mảng đầu vào:$[2, 4, 8, 16]$| Yếu tố | Sản xuất dây chuyền chẵn | Đặt sau khi xử lý | 
| --- | --- | --- | 
| 2 | 2 | {2} | 
| 4 | 4, 2 | {2, 4} | 
| 8 | 8, 4, 2 | {2, 4, 8} | 
| 16 | 16, 8, 4, 2 | {2, 4, 8, 16} | 

Câu trả lời cuối cùng là 4. 

Điều này thể hiện hành vi trong trường hợp xấu nhất khi các chuỗi chồng chéo hoàn toàn nhưng vẫn đóng góp các cấp độ khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi số được chia cho hai cho đến khi trở thành số lẻ, đóng góp các bước logarit | 
| Không gian |$O(n \log A)$| Trong trường hợp xấu nhất, tất cả các giá trị chẵn trung gian được lưu trữ trong một tập hợp | 

Tổng kích thước đầu vào tối đa là$2 \cdot 10^5$và mỗi số tạo ra tối đa khoảng 30 bước giảm một nửa kể từ$A \le 10^9$. Điều này làm cho giải pháp trở nên hiệu quả một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            seen = set()
            for x in a:
                while x % 2 == 0:
                    seen.add(x)
                    x //= 2
            out.append(str(len(seen)))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
6
40 6 40 3 20 1
1
1024
4
2 4 8 16
3
3 1 7
""") == """4
10
4
0"""

# custom cases
assert run("""1
1
1
""") == "0"

assert run("""1
3
2 2 2
""") == "1"

assert run("""1
3
6 10 14
""") == "3"

assert run("""1
2
12 3
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, [1] | 0 | đã kỳ quặc rồi | 
| 2,2,2 | 1 | trùng lặp không tăng hoạt động | 
| 6,10,14 | 3 | chuỗi rời rạc | 
| 12,3 | 2 | hành vi chẵn và lẻ hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng chỉ chứa các số lẻ. Trong tình huống đó, vòng lặp không bao giờ thực thi và tập hợp vẫn trống, tạo ra các hoạt động bằng 0 một cách chính xác. 

Một trường hợp khác là khi tất cả các số đều là lũy thừa của hai, chẳng hạn như$[1024]$. Dây chuyền trở thành$1024 \to 512 \to \dots \to 2$. Thuật toán thu thập mọi giá trị chẵn trung gian và mỗi cấp độ tương ứng với một thao tác cần thiết. Điều này đảm bảo câu trả lời bằng số mũ của lũy thừa lớn nhất của 2 ở đầu vào. 

Cuối cùng, lặp lại các giá trị giống hệt nhau như$[6, 6, 6]$chỉ đóng góp một chuỗi duy nhất$\{6, 3\}$. Vì 3 là số lẻ nên chỉ có 6 được tính và bộ này đảm bảo chúng ta không đếm quá nhiều công việc lặp lại.
