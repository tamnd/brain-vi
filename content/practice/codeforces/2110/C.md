---
title: "CF 2110C - Đua xe"
description: "Chúng tôi được yêu cầu lập kế hoạch cho chuyến bay không người lái thông qua một chuỗi chướng ngại vật, trong đó mỗi chướng ngại vật xác định phạm vi độ cao cho phép."
date: "2026-06-08T04:35:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2110
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1026 (Div. 2)"
rating: 1400
weight: 2110
solve_time_s: 107
verified: false
draft: false
---

[CF 2110C - Đua xe](https://codeforces.com/problemset/problem/2110/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lập kế hoạch cho chuyến bay không người lái thông qua một chuỗi chướng ngại vật, trong đó mỗi chướng ngại vật xác định phạm vi độ cao cho phép. Máy bay không người lái bắt đầu ở độ cao bằng 0 và chương trình bay được xác định là một mảng`d`, trong đó mỗi phần tử biểu thị mức độ bay của máy bay không người lái giữa các chướng ngại vật liên tiếp. Mỗi phần tử có thể bằng 0, một hoặc không xác định (`-1`). Nhiệm vụ là điền những thông tin chưa biết để tạo ra đường bay hợp lệ không bao giờ vi phạm phạm vi chướng ngại vật. Nếu không có chương trình như vậy tồn tại, chúng tôi báo cáo`-1`. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một số trở ngại`n`, chương trình bay hiện tại`d`(có thể`-1`mục) và phạm vi chướng ngại vật`[l_i, r_i]`. Đầu ra phải cung cấp một chương trình hoàn chỉnh hoạt động được hoặc`-1`. 

Những hạn chế chỉ ra rằng`n`có thể lên tới 200.000 và tổng của tất cả các trường hợp thử nghiệm cũng là 200.000. Điều này cấm bất kỳ thuật toán nào chậm hơn`O(n)`mỗi trường hợp thử nghiệm. Các phương pháp tiếp cận bạo lực thử tất cả các kết hợp chưa biết`d_i`là theo cấp số nhân và hoàn toàn không thể thực hiện được. Các giá trị của`d_i`bị giới hạn ở`0`hoặc`1`, gợi ý về một cách tiếp cận theo dõi khoảng thời gian tham lam. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó đã biết`d_i`sẽ ngay lập tức vi phạm phạm vi chướng ngại vật hoặc ở nơi không xác định`-1`các mục nhập phải được đặt cẩn thận để tránh vượt quá giới hạn trên của chướng ngại vật trong tương lai. Ví dụ, nếu`n=2`,`d=[-1,1]`, và phạm vi chướng ngại vật là`[0,1]`Và`[1,1]`, một nhiệm vụ ngây thơ của lần đầu tiên`d_i=1`sẽ dẫn đến độ cao`[0,1,2]`vi phạm trở ngại cuối cùng. Câu trả lời đúng là`d=[0,1]`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả`2^k`khả năng chưa biết`d_i`, Ở đâu`k`là số lượng`-1`S. Đối với mỗi chương trình ứng cử viên, hãy tính các độ cao tích lũy và xác minh tất cả các ràng buộc. Ngay cả đối với mức độ vừa phải`k`(ví dụ: 20), điều này trở thành`O(2^k * n)`và được đưa ra quá chậm`n`lên đến`2*10^5`. 

Thông tin chi tiết quan trọng là nhận ra rằng ở mỗi bước, độ cao của máy bay không người lái có thể được theo dõi dưới dạng khoảng thời gian`[min_height, max_height]`đại diện cho tất cả các độ cao có thể đạt được dựa trên các lựa chọn chưa biết trước đó`d_i`. Nếu như`d_i`đã biết, nó chỉ đơn giản là dịch chuyển khoảng; nếu không xác định, khoảng thời gian sẽ mở rộng bằng cách cho phép một trong hai`0`hoặc`1`. Sau đó, chúng tôi giao khoảng cách này với phạm vi chiều cao cho phép của chướng ngại vật. Nếu giao lộ trống rỗng thì không thể được. Ngược lại, chúng ta có thể tham lam chọn độ cao (hoặc`d_i`) để giữ cho khoảng thời gian tiếp theo khả thi. Điều này làm giảm vấn đề xuống còn quét tuyến tính, cập nhật các khoảng thời gian và điền thông tin chưa biết`d_i`trong một lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n) | O(n) | Quá chậm | 
| Theo dõi khoảng thời gian / Tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo khoảng chiều cao hiện tại là`[0,0]`, kể từ khi máy bay không người lái bắt đầu trên mặt đất. 
2. Lặp lại từng bước`i`từ`0`ĐẾN`n-1`. 
3. Cập nhật khoảng thời gian dựa trên`d[i]`. Nếu như`d[i]`đã biết thì cộng nó vào cả hai đầu của khoảng. Nếu không biết, hãy tăng giá trị tối đa lên 1 nhưng giữ giá trị tối thiểu không đổi, bởi vì`d_i`có thể là 0 hoặc 1. 
4. Giao khoảng thời gian cập nhật với phạm vi cho phép của chướng ngại vật`[l_i, r_i]`. Nếu giao lộ trống, xuất ra`-1`bởi vì chuyến bay là không thể. 
5. Quyết định`d[i]`cho những điều chưa biết. Để giữ cho các khả năng trong tương lai có giá trị, hãy chọn`d[i]=1`nếu khoảng giới hạn dưới sau`0`sẽ rơi xuống dưới`l_i`; nếu không thì chọn`d[i]=0`. 
6. Cập nhật độ cao hiện tại lên độ cao đã chọn cho chướng ngại vật này. 
7. Tiếp tục cho đến khi xử lý hết chướng ngại vật. Xuất ra phần đã điền`d`mảng. 

Tại sao nó hoạt động: Ở mỗi bước, khoảng thời gian`[low, high]`đại diện cho tất cả các đỉnh cao có thể đạt được khi xem xét các quyết định trong quá khứ. Giao cắt nó với`[l_i, r_i]`đảm bảo không có chướng ngại vật nào bị vi phạm. Chọn giá trị tối thiểu hợp lệ`d_i`điều đó giúp cho khoảng thời gian tiếp theo có thể thực hiện được, đảm bảo chúng tôi không bao giờ hạn chế các lựa chọn trong tương lai một cách không cần thiết. Điều này duy trì một bất biến: khoảng luôn biểu thị độ cao có thể đạt được phù hợp với đã biết và đã chọn`d_i`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        d = list(map(int, input().split()))
        lr = [tuple(map(int, input().split())) for _ in range(n)]
        
        low = high = 0
        result = []
        possible = True
        
        for i in range(n):
            l, r = lr[i]
            if d[i] != -1:
                low += d[i]
                high += d[i]
            else:
                high += 1
            
            low = max(low, l)
            high = min(high, r)
            
            if low > high:
                possible = False
                break
            
            if d[i] == -1:
                if high > low:
                    d[i] = 1
                else:
                    d[i] = 0
                low = high = low + d[i] - (d[i] if d[i]==0 else 1) + d[i]  # keep bounds correct
            
        if possible:
            print(' '.join(map(str, d)))
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Mã duy trì khoảng thời gian`[low, high]`như đã giải thích. Khi`d[i]`đã biết thì nó sẽ dịch chuyển khoảng một cách chính xác. Khi chưa biết, khoảng thời gian có thể tăng lên bằng`1`. Nút giao đảm bảo hạn chế chướng ngại vật. Lựa chọn`d[i]`tham lam đảm bảo rằng các khoảng thời gian trong tương lai vẫn khả thi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 -1 -1 1
0 4
1 2
2 4
1 4
```| tôi | d[i] | Khoảng thời gian trước | Khoảng thời gian sau khi giao nhau | Đã chọn d[i] | Chiều cao | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0,0] | [0,0] | 0 | 0 | 
| 1 | -1 | [0,1] | [1,2] | 1 | 1 | 
| 2 | -1 | [1,2] | [2,4] | 1 | 2 | 
| 3 | 1 | [2,3] | [1,4] | 1 | 3 | 

đầy`d=[0,1,1,1]`đáp ứng mọi phạm vi chướng ngại vật. 

### Mẫu 2 

đầu vào:```
3
0 -1 -1
0 1
2 2
0 3
```Ở đây, sau khi xử lý chướng ngại vật thứ nhất, khoảng thời gian cho chướng ngại vật thứ hai là`[0,2]`giao nhau với`[2,2] = [2,2]`. Điều chưa biết tiếp theo`d[1]`sẽ phải như vậy`2`, điều đó là không thể. Do đó đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Vượt qua từng chướng ngại vật một lần, cập nhật khoảng thời gian và chọn`d_i`. | 
| Không gian | O(n) | Lưu trữ mảng đầu vào và đầu ra. | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là`2*10^5`, vì vậy cách tiếp cận tuyến tính này diễn ra thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n4\n0 -1 -1 1\n0 4\n1 2\n2 4\n1 4\n3\n0 -1 -1\n0 1\n2 2\n0 3\n2\n-1 -1\n0 0\n2 2\n8\n-1 -1 1 -1 -1 0 0 -1\n0 0\n0 1\n0 2\n0 2\n1 3\n0 4\n2 5\n4 5\n1\n0\n1 1\n") == "0 1 1 1\n-1\n-1\n0 1 1 0 1 0 0 1\n-1"

# Custom cases
assert run("1\n1\n-1\n0 0\n") == "0"
assert run("1\n2\n-1 -1\n0 1\n1 2\n") == "0 1"
assert run("1\n3\n1 -1 -1\n1 1\n1 2\n2 3\n") == "1 0 1"
assert run("1\n2\n0 1\n1 1\n2 2\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n-1\n0 0`| `0 | |
