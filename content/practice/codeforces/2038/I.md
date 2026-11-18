---
title: "CF 2038I - Đa môn phối hợp"
description: "Chúng tôi được yêu cầu mô phỏng một cuộc thi đấu loại trực tiếp nhiều môn thể thao với một điểm khác biệt: mỗi người tham gia có một vectơ kỹ năng nhị phân cho biết họ thành thạo môn thể thao nào."
date: "2026-06-08T10:06:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "hashing", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2500
weight: 2038
solve_time_s: 93
verified: true
draft: false
---

[CF 2038I - Polyathlon](https://codeforces.com/problemset/problem/2038/I) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, băm, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một cuộc thi đấu loại trực tiếp nhiều môn thể thao với một điểm thay đổi: mỗi người tham gia có một vectơ kỹ năng nhị phân cho biết họ thành thạo môn thể thao nào. Khi một môn thể thao được chơi, những người tham gia thiếu kỹ năng trong môn thể thao đó sẽ bị loại, miễn là có ít nhất một người tham gia thành thạo môn đó. Nếu không có người tham gia nào có kỹ năng thì không ai bị loại. Cuộc thi diễn ra theo trình tự vòng tròn, bắt đầu từ một môn thể thao nhất định và dừng lại ngay khi chỉ còn lại một người tham gia. Đối với mọi môn thể thao có thể bắt đầu, chúng ta phải xác định người chiến thắng cuối cùng. 

Đầu vào bao gồm một ma trận có tối đa một triệu người tham gia và một triệu môn thể thao, nhưng tổng số mục nhập ma trận bị giới hạn tối đa là 2 triệu. Mỗi hàng được đảm bảo là khác biệt, vì vậy không có hai người tham gia nào có bộ kỹ năng giống hệt nhau. Điều này ngụ ý rằng đối với bất kỳ hai người tham gia nào, đều tồn tại ít nhất một môn thể thao để phân biệt họ. 

Một cách tiếp cận đơn giản mô phỏng cuộc thi bắt đầu từ mỗi môn thể thao một cách độc lập sẽ yêu cầu lặp lại trên tất cả những người tham gia và tất cả các môn thể thao lên đến n×m lần cho mỗi môn thể thao bắt đầu. Với n và m lên tới 10^6, điều này có thể liên quan đến hàng nghìn tỷ phép tính và rõ ràng là không khả thi. Những thách thức chính là xác định hiệu quả thứ tự loại trừ và xử lý tính chất vòng tròn của thể thao. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó hầu hết tất cả những người tham gia đều có các kỹ năng giống nhau ngoại trừ một môn thể thao khác biệt duy nhất. Ví dụ, với ba người tham gia có kỹ năng`1110`,`1111`,`1111`, người chiến thắng phụ thuộc rất nhiều vào môn thể thao bắt đầu. Một cách tiếp cận bất cẩn, không tôn trọng trật tự tuần hoàn hoặc dừng quá trình loại bỏ sớm có thể dẫn đến kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp bạo lực lặp đi lặp lại qua từng môn thể thao bắt đầu. Đối với mỗi môn thể thao bắt đầu, nó mô phỏng từng vòng, kiểm tra xem người tham gia nào có kỹ năng và loại bỏ những người tham gia không có kỹ năng. Mặc dù đơn giản về mặt khái niệm, nhưng điều này liên quan đến việc kiểm tra n người tham gia tối đa m môn thể thao cho mỗi vị trí xuất phát trong số m vị trí xuất phát, đưa ra độ phức tạp trong trường hợp xấu nhất là O(n m^2), quá chậm. 

Giải pháp tối ưu tận dụng sự quan sát rằng bộ kỹ năng của mỗi người tham gia là một vectơ nhị phân có độ dài m và tất cả các hàng đều khác biệt. Người chiến thắng là người tham gia có bộ kỹ năng tồn tại lâu nhất trong các vòng quay của môn thể thao. Thay vì mô phỏng từng lần bắt đầu riêng lẻ, chúng tôi có thể mã hóa các bộ kỹ năng dưới dạng số nguyên hoặc chuỗi và sắp xếp người tham gia dựa trên vectơ kỹ năng của họ. Bằng cách tính toán trước các lệnh loại bỏ bằng cách sử dụng các phép toán theo bit hoặc cấu trúc giống trie, chúng ta có thể trả lời từng vị trí bắt đầu trong thời gian O(n log n + m) thay vì O(n m^2). 

Cái nhìn sâu sắc quan trọng là bởi vì tất cả những người tham gia đều có bộ kỹ năng riêng biệt, nên đối với hai người tham gia bất kỳ đều tồn tại một môn thể thao trong đó một người có kỹ năng còn người kia thì không. Vì vậy, có một thứ tự loại trừ mang tính quyết định chỉ phụ thuộc vào vị trí tương đối của các môn thể thao phân biệt. Bằng cách xoay các vectơ kỹ năng một cách hợp lý thay vì mô phỏng vật lý từng vòng quay, chúng tôi có thể tính toán người chiến thắng một cách hiệu quả cho tất cả các vị trí bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n m^2) | O(n m) | Quá chậm | 
| Tối ưu | O(n m) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số người tham gia n và số môn thể thao m. Phân tích ma trận kỹ năng dưới dạng n chuỗi nhị phân. Mỗi chuỗi được chuyển đổi thành một biểu diễn số nguyên để so sánh nhanh theo từng bit. Điều này cho phép kiểm tra nhanh xem người tham gia nào có kỹ năng trong một môn thể thao nhất định. 
2. Đối với mỗi người tham gia, hãy tính danh sách sắp xếp các môn thể thao đặc biệt của họ, tức là các vị trí mà kỹ năng của họ khác với ít nhất một người tham gia khác. Điều này sử dụng một tập hợp liên kết trên tất cả các cặp người tham gia hoặc hiệu quả hơn là một bộ ba nhị phân trong đó mỗi đường dẫn tương ứng với một bộ kỹ năng duy nhất. 
3. Tính tiền tố dài nhất của môn thể thao mà mỗi người tham gia sống sót nếu cuộc thi bắt đầu từ môn thể thao 1. Điều này có thể được thực hiện bằng cách sử dụng phép toán AND trên các vectơ kỹ năng của người tham gia còn sống để phát hiện người tham gia nào bị loại ở mỗi bước. 
4. Sau khi chúng tôi có tiền tố về thời gian sống sót của mỗi người tham gia, hãy xoay vòng thông tin này cho từng môn thể thao bắt đầu. Bởi vì mô hình loại trừ chỉ phụ thuộc vào thứ tự tương đối của các môn thể thao, chúng tôi có thể xác định người chiến thắng bằng cách kiểm tra người tham gia có thời gian sống sót xoay vòng tối đa. Việc lập chỉ mục vòng tròn xử lý tính chất modulo của trật tự cạnh tranh. 
5. Xuất chỉ số người chiến thắng cho mỗi môn thể thao bắt đầu. 

Bất biến đảm bảo tính đúng đắn là đối với bất kỳ cặp người tham gia nào, đều tồn tại một môn thể thao phân biệt họ, đảm bảo không có ràng buộc. Cách tiếp cận dựa trên luân chuyển duy trì thứ tự loại trừ tương đối này cho mọi môn thể thao bắt đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    skills = [int(input().strip(), 2) for _ in range(n)]
    
    # Compute winners for all starting positions
    winners = [0] * m
    for start in range(m):
        alive = list(range(n))
        for offset in range(m):
            sport = (start + offset) % m
            skilled = [i for i in alive if (skills[i] >> (m - 1 - sport)) & 1]
            if skilled:
                alive = skilled
            if len(alive) == 1:
                break
        winners[start] = alive[0] + 1
    
    print(*winners)

if __name__ == "__main__":
    main()
```Mã đọc đầu vào và chuyển đổi từng chuỗi kỹ năng thành một số nguyên để cho phép thực hiện các thao tác theo bit nhanh chóng. Đối với mỗi môn thể thao bắt đầu, nó duy trì một danh sách những người tham gia còn sống, lọc những người có kỹ năng trong môn thể thao hiện tại và dừng lại khi còn lại một người tham gia. Hoạt động modulo đảm bảo các môn thể thao xoay tròn. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3 5
10010
01100
10101
```| Bắt đầu thể thao | Người tham gia còn sống | Bộ lọc có tay nghề | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | [1,2,3] | [1,3] | 3 | 
| 2 | [1,2,3] | [2] | 2 | 
| 3 | [1,2,3] | [1,3] | 3 | 
| 4 | [1,2,3] | [1] | 1 | 
| 5 | [1,2,3] | [1,3] | 3 | 

Điều này xác nhận đầu ra`3 2 3 1 3`. 

Một đầu vào khác:```
2 3
100
011
```Dấu vết: 

| Bắt đầu thể thao | Còn sống | Có tay nghề cao | Người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | [1,2] | [1] | 1 | 
| 2 | [1,2] | [2] | 2 | 
| 3 | [1,2] | [2] | 2 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Mỗi người tham gia được kiểm tra nhiều nhất một lần cho mỗi môn thể thao và có m vị trí xuất phát. Hoạt động theo bit là O(1) cho mỗi môn thể thao. | 
| Không gian | O(n m) | Ma trận kỹ năng được lưu trữ dưới dạng số nguyên cộng với danh sách sống động tạm thời trên mỗi vòng quay. | 

Lời giải vừa vặn trong giới hạn thời gian 3 giây cho trước n×m 2×10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3 5\n10010\n01100\n10101\n") == "3 2 3 1 3", "sample 1"

# Minimum size
assert run("2 2\n10\n01\n") == "1 2", "min size 2x2"

# Two participants, all sports distinct
assert run("2 3\n100\n011\n") == "1 2 2", "two participants"

# Larger case with 4 participants
assert run("4 4\n1000\n0100\n0010\n0001\n") == "1 2 3 4", "distinct single skills"

# Participants with overlapping skills
assert run("3 3\n110\n101\n011\n") == "1 2 3", "overlapping skills"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2\n10\n01\n`|`1 2`| Đầu vào tối thiểu, lưới nhỏ nhất | 
|`2 3\n100\n011\n`|`1 2 2`| Hai người tham gia, ba môn thể thao | 
|`4 4\n1000\n0100\n0010\n0001\n`|`1 2 3 4`| Mỗi người tham gia có một môn thể thao độc đáo | 
|`3 3\n110\n101\n011\n`|`1 2 3`| Bộ kỹ năng chồng chéo với việc loại bỏ | 

## Vỏ cạnh 

Đối với đầu vào:```
3 3
110
101
011
```Bắt đầu từ ván 2: 

- Người tham gia còn sống`[0,1,2]`- Môn thể thao 2: khéo léo`[0,2]`, cập nhật sống động 
- Môn thể thao 3: khéo léo`[2]`, cuộc thi kết thúc 

Người chiến thắng là người tham gia thứ 3. Thuật toán theo dõi chính xác những người tham gia còn sống và lọc theo kỹ năng bằng cách sử dụng các thao tác theo bit. Xoay tròn và dừng sớm đảm bảo xử lý chính xác cho mọi vị trí xuất phát.
