---
title: "CF 1738C - Người nghiện số chẵn"
description: "Lỗi không nằm ở cơ chế I/O hay lặp lại, mà nằm ở logic của ý nghĩa thực sự của “ô biệt lập”. Giải pháp trước đó ngầm giả định phân loại toàn cầu chỉ dựa trên kích thước bảng và sau đó được mặc định trả về $(1,1)$ cho tất cả các trường hợp."
date: "2026-06-09T17:50:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1738
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 22"
rating: 1500
weight: 1738
solve_time_s: 410
verified: false
draft: false
---

[CF 1738C - Người nghiện số chẵn](https://codeforces.com/problemset/problem/1738/C) 

**Đánh giá:** 1500 
**Tags:** dp, trò chơi, tham lam, toán học 
**Thời gian giải:** 6 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Chẩn đoán 

Lỗi không nằm ở cơ chế I/O hay lặp lại, mà nằm ở logic của ý nghĩa thực sự của “ô biệt lập”. 

Giải pháp trước đó ngầm giả định phân loại toàn cầu chỉ dựa trên kích thước bảng và sau đó được mặc định trả về$(1,1)$cho mọi trường hợp. Điều đó mâu thuẫn với định nghĩa thực tế: sự cô lập là một thuộc tính cục bộ của một ô cụ thể, tùy thuộc vào việc tất cả hiệp sĩ di chuyển từ ô đó có ở trong bàn cờ hay không. 

Kết quả đầu ra dự kiến ​​cho thấy các ô biệt lập tồn tại theo những cách có cấu trúc không cần thiết. Ví dụ, trên một$3 \times 3$lên trung tâm$(2,2)$bị cô lập, nhưng các góc không nhất thiết phải được coi là bị cô lập theo cách tương tự theo cách giải thích dự định của bài toán. Tương tự, trên các bảng lớn hơn như$8 \times 8$, tồn tại những vị trí biệt lập cụ thể như$(7,2)$. 

Vì vậy, nhiệm vụ chính xác thực sự là xác minh hình học trên từng ô chứ không phải là một phím tắt chung. 

Lỗi ở đầu ra hiện tại phát sinh do thuật toán đã thu gọn toàn bộ lưới thành một câu trả lời không đổi mà không kiểm tra khả năng tiếp cận hiệp sĩ thực tế. 

## Lập luận đúng 

Một tế bào$(r,c)$bị cô lập nếu tất cả tám nước đi của hiệp sĩ đều nằm ngoài bàn cờ. Điều kiện đó hoàn toàn mang tính cục bộ và phải được kiểm tra rõ ràng. 

Bảng đủ nhỏ để chúng ta có thể tấn công vũ phu tất cả các ô một cách an toàn. Đối với mỗi ô, chúng tôi kiểm tra tám bước di chuyển có thể xảy ra và xác minh xem có bất kỳ bước di chuyển nào nằm trong lưới hay không. Nếu không có, chúng tôi xuất ô đó ngay lập tức. 

Nếu không có ô như vậy tồn tại, chúng tôi xuất ra bất kỳ ô hợp lệ nào, ví dụ:$(1,1)$. 

Điều này trực tiếp phù hợp với định nghĩa và tránh những giả định không chính xác về cấu trúc hội đồng quản trị. 

## Giải pháp Python đã được sửa```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    moves = [(2, 1), (2, -1), (-2, 1), (-2, -1),
             (1, 2), (1, -2), (-1, 2), (-1, -2)]
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        found = False
        
        for r in range(1, n + 1):
            for c in range(1, m + 1):
                isolated = True
                
                for dr, dc in moves:
                    nr, nc = r + dr, c + dc
                    if 1 <= nr <= n and 1 <= nc <= m:
                        isolated = False
                        break
                
                if isolated:
                    print(r, c)
                    found = True
                    break
            
            if found:
                break
        
        if not found:
            print(1, 1)

if __name__ == "__main__":
    solve()
```## Tại sao điều này khắc phục được sự cố 

Giải pháp đã sửa sẽ khôi phục định nghĩa trực tiếp về sự cô lập: một ô hợp lệ khi và chỉ khi tất cả các nước đi của hiệp sĩ rời khỏi bàn cờ. Điều này tránh mọi lý luận tổng thể không chính xác và đảm bảo rằng các trường hợp đặc biệt như$(3,3)$lưới hoặc bảng không đối xứng được xử lý chính xác. 

Việc thoát sớm đảm bảo chúng tôi vẫn trả về một ô biệt lập hợp lệ ngay khi tìm thấy một ô, phù hợp với yêu cầu rằng bất kỳ ô nào như vậy đều được chấp nhận. 

## Phân tích độ phức tạp 

Mỗi trường hợp thử nghiệm kiểm tra tối đa$64$các ô và mỗi ô sẽ kiểm tra nhiều nhất$8$di chuyển, do đó tổng công việc cho mỗi trường hợp kiểm thử là không đổi. Điều này dễ dàng phù hợp với những hạn chế. 

## Bài học chính 

Sai lầm chính là thay thế điều kiện hình học cục bộ bằng một phím tắt tổng thể. Trong các bài toán lưới có ràng buộc nhỏ, mô phỏng trực tiếp thường là phương pháp chính xác và an toàn nhất, đặc biệt khi định nghĩa phụ thuộc vào tập kề cận hoặc tập di chuyển.
