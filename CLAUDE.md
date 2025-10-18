# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based blog site for Japanese food content. It's built on the Astro Blog starter template with support for Markdown/MDX content, SEO features, and static site generation.

## Development Commands

Run from the project root:

- `npm run dev` - Start development server at localhost:4321
- `npm run build` - Build production site to ./dist/
- `npm run preview` - Preview production build locally
- `npm run astro ...` - Run Astro CLI commands (e.g., `astro check`)

## Architecture

### Content Management

The blog uses Astro's Content Collections API (v5):

- **Content Schema** (src/content.config.ts): Defines the blog collection with Zod schema validation for frontmatter
  - Required fields: title, description, pubDate
  - Optional fields: updatedDate, heroImage (using Astro's image() helper)

- **Content Location**: All blog posts stored in src/content/blog/ as .md or .mdx files

- **Dynamic Routes**: src/pages/blog/[...slug].astro uses `getCollection('blog')` to generate static pages for each post
  - `getStaticPaths()` returns all posts with slug params mapped to post.id
  - Uses `render()` to get the Content component for each post

### Site Configuration

- **Global constants** (src/consts.ts): Centralized location for SITE_TITLE and SITE_DESCRIPTION
- **Astro config** (astro.config.mjs): MDX and sitemap integrations enabled, site URL set to https://example.com

### Layout System

- **BlogPost layout** (src/layouts/BlogPost.astro): Main blog post template
  - Accepts CollectionEntry<'blog'>['data'] props
  - Includes hero image support with Astro's Image component
  - Contains scoped styles for prose content (max-width: 720px)
  - Uses slot for post content injection

### Components

Standard Astro components in src/components/:
- BaseHead.astro - SEO meta tags
- Header.astro, Footer.astro - Layout components
- FormattedDate.astro - Date formatting utility
- HeaderLink.astro - Navigation links

## Styling

No CSS framework used. Styling is handled through:
- Global CSS in layouts
- Scoped `<style>` blocks in .astro components
- CSS custom properties (e.g., --gray-dark, --box-shadow)

## 블로그 콘텐츠 작성 규칙

이 블로그는 **수익화를 목적으로 한 일식 전문 블로그**입니다.

### 글 작성 명령어

사용자가 **"새 글 작성"**이라고 말하면, 아래 규칙에 따라 자동으로 새로운 블로그 포스트를 작성합니다.

### 작성 규칙

1. **SEO 최적화**
   - 검색 엔진에 유리한 제목과 메타 설명 작성
   - 키워드를 자연스럽게 본문에 배치
   - 적절한 제목 계층 구조 사용 (h2, h3)

2. **주제 선정**
   - 매번 **새로운 일식 주제**로 작성 (스시, 라멘, 돈카츠, 우동, 사시미, 일본 요리 문화, 조리 팁, 맛집 리뷰 등)
   - 이미 작성된 주제와 중복되지 않도록 확인

3. **분량**
   - **최소 5000자 이상** 작성
   - 깊이 있고 유익한 정보 제공

4. **작성 스타일**
   - **자연스러운 말투** 사용 (AI 느낌이 나지 않게)
   - 개인적인 경험이나 의견을 적절히 포함
   - 독자와 소통하는 듯한 친근한 톤

5. **이미지 삽입**
   - **Unsplash**에서 작성 중인 일식과 관련된 사진 검색
   - 글 중간에 **최소 1개 이상** 삽입
   - 이미지는 `![설명](https://images.unsplash.com/...)` 형식으로 삽입

6. **파일 형식**
   - `.md` (마크다운) 형식으로 작성
   - 파일명: `src/content/blog/주제-영문.md` (예: `sushi-guide.md`)
   - Frontmatter 필수 항목:
     ```yaml
     ---
     title: '제목'
     description: 'SEO 최적화된 설명 (150자 이내)'
     pubDate: '현재 날짜'
     heroImage: 'Unsplash 이미지 URL'
     ---
     ```

### 작성 예시

사용자: "새 글 작성"

→ Claude가 자동으로:
1. 새로운 일식 주제 선정
2. SEO 최적화된 제목/설명 생성
3. 5000자 이상 자연스러운 본문 작성
4. Unsplash에서 관련 이미지 검색 및 삽입
5. 적절한 파일명으로 마크다운 파일 생성
